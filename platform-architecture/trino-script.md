# Trino Script

This script, which creates the iceberg view, contains five methods

* build\_discord: Creating a discord entity table
* build\_twitter: Creating a twitter entity table
* discord\_view\_update: Updating the discord view in iceberg
* twitter\_view\_update: Updating the twitter view in iceberg
* create\_standard\_view: Creating a view of a web2 standard table



code:

```python
import pandas as pd
import pydash
from trino.dbapi import connect
from trino.auth import BasicAuthentication

is_prod = True

def get_conn():
    cfg = {
        'host': 'prod_host' if is_prod else 'staging_host',
        'port': '443',
        'user': '',
        'password': '',
        'catalog': 'iceberg',
        'schema': 'animoca',
    }
    conn = connect(
        host=cfg['host'],
        port=cfg['port'],
        user=cfg['user'],
        auth=BasicAuthentication(cfg['user'], cfg['password']),
        http_scheme="https",
        catalog=cfg['catalog'],
        schema=cfg['schema']
    )
    return conn


def query_trino_admin(query_string: str):
    conn = get_conn()
    cur = conn.cursor()
    cur.execute(query_string)
    rows = cur.fetchall()
    result = rows
    cur.close()
    conn.close()
    return result


def query_trino_2_df(query_string: str):
    conn = get_conn()
    cur = conn.cursor()
    cur.execute(query_string)
    columns = []
    for i in range(len(cur.description)):
        columns.append(cur.description[i][0])
    try:
        rows = cur.fetchall()
        result = pd.DataFrame(rows, columns=columns)
        # result = rows
        cur.close()
        conn.close()
        return result
    except Exception as e:
        print(e)
        return None


"""{Project: [Server]}"""
project_discord_server_mapping = {
    'Gamee': ['Arc8'],
    'Anichess': ['Anichess'],
    'Mocaverse': ['Mocaverse'],
    'Motorverse': ['Motorverse'],
    'Open Campus': ['Open Campus'],
    'Phantom Galaxies': ['Phantom Galaxies'],
    'Sandbox': ['Sandbox']
}


"""'Project': [['Handler', 'HandlerTableName']]"""
project_twitter_handle_mapping = {
    'Mocaverse': [['MocaverseNFT', 'mocaversenft']],
    'Gamee': [
        ['ARC8App', 'arc8app'],
        ['GAMEEToken', 'gameetoken'],
        ['WatBird', 'watbird'],
    ],  # Arc8 -> Gamee
    'Open Campus': [['opencampus_xyz','opencampus_xyz']],
    'Phantom Galaxies': [['the_phantom_g','the_phantom_g']],
    'Anichess': [['AnichessGame', 'anichessgame']],
    'Sandbox': [['TheSandboxGame', 'thesandboxgame']],
    'Motorverse': [['TheMotorverse', 'themotorverse']],
    'Pixelmon': [['Pixelmon','pixelmon']],
    'Gumble': [['gomblegames', 'gomblegames']],
    'Crazy Defense Heroes': [['TowerToken', 'towertoken']],
    'AB Marketing': [['animocabrands', 'animocabrands']],
    'ApeCoin': [
        ['apecoin', 'ape_coin'],
        ['othersidemeta', 'othersidemeta'],
        ['boredapeyc', 'boredapeyc']
    ],
}

def create_stander_view(web2_project):
    """
    Create stander view for project
    web2_project: str, You can find the list of projects with this query: select * from iceberg.animoca.project_mapping
    """
    table_list = [
        'asset_flow', 'account_mapping', 'session_event', 'user', 'user_event', 'user_session_daily_stats'
    ]
    for table_name in table_list:
        stander_view_sql = f"""
        CREATE or replace VIEW iceberg.animoca.{pydash.snake_case(web2_project)}__{table_name} SECURITY INVOKER AS
        SELECT *
        FROM
          iceberg.animoca.{table_name}
        WHERE (project = '{web2_project}')
        """
        print(stander_view_sql)
        query_trino_admin(stander_view_sql)

def discord_view_update():
    """
     update standard discord view, depend on project_server_mapping.
     If there is a new project joining, you can add a mapping relationship to the project_server_mapping.
    """
    table_list = [
        'messages', 'message_engagement', 'discord_channel_mapping', 'discord_member_count',
        # 'discord_members' -- only Open Campus has this table
    ]
    for table_name in table_list:
        sub_sql_list = []
        for project, server_id_list in project_discord_server_mapping.items():
            table_schema = f'animoca_{pydash.snake_case(project)}'
            for server_id in server_id_list:
                server_id_name = pydash.snake_case(server_id)
                sub_table_sql = f"""
select '{project}' as project, '{server_id}' as server_id, * from raw_data.{table_schema}.{server_id_name}_{table_name}
                """
                sub_sql_list.append(sub_table_sql)
        union_sql = '\nUNION ALL\n'.join(sub_sql_list)
        table_name = table_name.replace('discord_', 'discord__raw__') if table_name.startswith('discord_') else f'discord__raw__{table_name}'
        view_sql = f"""
    CREATE OR REPLACE VIEW iceberg.animoca.{table_name} AS
    select * from ({union_sql})
        """
        print('====================================')
        print(view_sql)
        query_trino_admin(view_sql)


def build_discord(project, server):
    """
        Create discord table for project
        project: str, You can find the list of projects with this query: select project_name from mysql.metabase.project_mapping
        server str, discord server name, you can find server name in s3 file path: social/discord/{server}/*
    """
    project = pydash.snake_case(project)
    server_name = pydash.snake_case(server)
    discord_channel_mapping_sql = f"""
    create table raw_data.animoca_{project}.{server_name}_discord_channel_mapping (
        id varchar,
        name varchar,
        on_date date,
        p__on_date date WITH ( partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    ) WITH (
      external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/discord/{server}/discord_channel_mapping',
      format = 'JSON',
      partition_projection_enabled = true,
      partitioned_by = ARRAY['p__on_date']
    )
    """
    query_trino_admin(discord_channel_mapping_sql)
    discord_member_count_sql = f"""
    create table raw_data.animoca_{project}.{server_name}_discord_member_count (
        date date,
        number_of_members bigint,
        p__on_date date WITH ( partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    ) WITH (
      external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/discord/{server}/discord_member_count',
      format = 'JSON',
      partition_projection_enabled = true,
      partitioned_by = ARRAY['p__on_date']
    )
    """
    query_trino_admin(discord_member_count_sql)


    query_trino_admin(f"""
    create table raw_data.animoca_{project}.{server_name}_message_engagement (
        date date,
        sentiment double,
        sentences bigint,
        slang bigint,
        engagement_score double,
        channel_id varchar,
        p__on_date date WITH ( partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    ) WITH (
      external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/discord/{server}/message_engagement',
      format = 'JSON',
      partition_projection_enabled = true,
      partitioned_by = ARRAY['p__on_date']
    )
    """)


    query_trino_admin(f"""
    CREATE TABLE raw_data.animoca_{project}.{server_name}_messages (
      id varchar,
      type bigint,
      content varchar,
      channel_id varchar,
      author ROW(id varchar, username varchar, avatar varchar, discriminator varchar, public_flags bigint, premium_type bigint, flags bigint, global_name varchar),
      mention_roles array(varchar),
      pinned boolean,
      mention_everyone boolean,
      tts boolean,
      timestamp varchar,
      flags bigint,
      reactions array(ROW(emoji ROW(name varchar), count bigint, count_details ROW(burst bigint, normal bigint), me_burst boolean, burst_me boolean, me boolean, burst_count bigint)),
      sentiment double,
      webhook_id varchar,
        p__on_date date WITH ( partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2011-01-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
      external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/discord/{server}/messages',
      format = 'JSON',
      partition_projection_enabled = true,
      partitioned_by = ARRAY['p__on_date']
    )
    """)



def build_twitter(project, handler_table_name):
    """
        Create twitter view for project
        project: str, You can find the list of projects with this query: select project_name from mysql.metabase.project_mapping
        handler_table_name: str, twitter handler name, you can find server name in s3 file path: social/twitter/{handler_table_name}/*
    """
    schema = f'raw_data.animoca_{pydash.snake_case(project)}'
    # user_detail
    user_detail_sql = f"""
    CREATE TABLE if not exists {schema}.{handler_table_name}_user_detail (
       id varchar,
       handler varchar,
       timestamp varchar,
       name varchar,
       sign_up_at varchar,
       description varchar,
       profile_image_url varchar,
       tweet_count bigint,
       listed_count bigint,
       following_count bigint,
       followers_count varchar,
       p__on_date date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/user_detail',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(user_detail_sql)
    query_trino_admin(user_detail_sql)


    twitter_tweet_metrics_sql = f"""
    CREATE TABLE {schema}.{handler_table_name}_twitter_tweet_metrics (
       handler varchar,
       tweet_id varchar,
       timestamp varchar,
       text varchar,
       type varchar,
       tweet_time varchar,
       like_count bigint,
       bookmark_count bigint,
       reply_count bigint,
       quote_count bigint,
       retweet_count bigint,
       impression_count bigint,
       sentiment double,
       sentiment_finbert ROW(sentiment varchar, probabilities row(neutral double, positive double, negative double)),
       p__on_date date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/tweet_with_metrics',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(twitter_tweet_metrics_sql)
    query_trino_admin(twitter_tweet_metrics_sql)


    mentions_sql = f"""
    CREATE TABLE {schema}.{handler_table_name}_mentions (
       handle varchar,
       author_id varchar,
       timestamp varchar,
       tweet_id varchar,
       text varchar,
       type varchar,
       tweet_time varchar,
       like_count bigint,
       bookmark_count varchar,
       reply_count bigint,
       quote_count bigint,
       retweet_count bigint,
       impression_count bigint,
       sentiment double,
       p__on_date date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/mentions',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(mentions_sql)
    query_trino_admin(mentions_sql)
    mentions_kols_sql = f"""
    CREATE TABLE {schema}.{handler_table_name}_mentions_kols (
       handle varchar,
       author_id varchar,
       timestamp varchar,
       tweet_id varchar,
       text varchar,
       type varchar,
       tweet_time varchar,
       like_count bigint,
       bookmark_count varchar,
       reply_count bigint,
       quote_count bigint,
       retweet_count bigint,
       impression_count bigint,
       sentiment double,
       p__on_date  date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/mentions_kols',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(mentions_kols_sql)
    query_trino_admin(mentions_kols_sql)
    reposts_sql = f"""
    CREATE TABLE {schema}.{handler_table_name}_reposts (
       handle varchar,
       author_id varchar,
       timestamp varchar,
       tweet_id varchar,
       text varchar,
       type varchar,
       tweet_time varchar,
       like_count bigint,
       bookmark_count varchar,
       reply_count bigint,
       quote_count bigint,
       retweet_count bigint,
       impression_count bigint,
       sentiment double,
       p__on_date  date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/reposts',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(reposts_sql)
    query_trino_admin(reposts_sql)
    reposts_kols_sql = f"""
    CREATE TABLE {schema}.{handler_table_name}_reposts_kols (
       handle varchar,
       author_id varchar,
       timestamp varchar,
       tweet_id varchar,
       text varchar,
       type varchar,
       tweet_time varchar,
       like_count bigint,
       bookmark_count varchar,
       reply_count bigint,
       quote_count bigint,
       retweet_count bigint,
       impression_count bigint,
       sentiment double,
       p__on_date  date WITH (partition_projection_format = 'yyyy-MM-dd', partition_projection_interval = 1, partition_projection_range = ARRAY['2024-04-01','NOW'], partition_projection_type = 'DATE' )
    )
    WITH (
       external_location = 's3a://web2-raw-data-{'prod' if is_prod else 'staging'}/social/twitter/{handler_table_name}/reposts_kols',
       format = 'JSON',
       partition_projection_enabled = true,
       partitioned_by = ARRAY['p__on_date']
    )
    """
    print(reposts_kols_sql)
    query_trino_admin(reposts_kols_sql)



def twitter_view_update():
    """
        Update the standard view for twitter data, depends on the project_twitter_handle_mapping.
        If there is a new project joining, you can add a mapping relationship to the project_twitter_handle_mapping.
    """
    table_list = [
        'twitter_tweet_metrics', 'user_detail',
        # 'mentions', 'mentions_kols', 'reposts', 'reposts_kols'  # not all handler have these tables
    ]
    for table_name in table_list:
        select_sql_list = []
        for key, value in project_twitter_handle_mapping.items():
            for handler in value:
                schema = f'animoca_{pydash.snake_case(key)}'
                select_sql = f"""
                select *, '{key}' as project  FROM raw_data.{schema}.{handler[1]}_{table_name}
                """
                select_sql_list.append(select_sql)
        select_sql_list.append(f"""
        SELECT
         *
       , cast(null as varchar) as project
       FROM
         raw_data.animoca_portco.portco_user_detail
        """)


        view_sql = f""" 
        CREATE or replace VIEW iceberg.animoca.twitter__raw__{table_name} SECURITY DEFINER AS
        SELECT *
        FROM
        (
        {' union all '.join(select_sql_list)}
        )
        WHERE (p__on_date > date '2024-04-10')
        """
        print(view_sql)
        query_trino_admin(view_sql)




if __name__ == '__main__':
    """
    Before executing the script, pay attention to the selected runtime environment.
    """
    print(f'is prod: {is_prod}')
    """example"""
    build_discord(project='Gamee', server='Arc8')
    build_twitter(project='Gamee', handler_table_name='ARC8App')
    discord_view_update()  # update discord raw view
    twitter_view_update()  # update twitter raw view
    create_stander_view(web2_project='Gamee TG')  # Gamee TG is the sub project of Gamee
```

\




\
\
\
\
\
