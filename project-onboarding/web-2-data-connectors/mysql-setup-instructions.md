# Mysql Setup Instructions



1. Select "Mysql" as the source type.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf3DQOhgMPCzzvEdXSgH2Y8s0z2ViW-xaQ_jY4xEP6VWNbwUZXYVXOtnlqmzrYcQ04wr-wDV4bfvASGSgjJbvZLP47lCdjA7c3jLxa1BMAohIvyBvohedhB1KOrBV3Zs5sIksJpFFcdSyCjNUA99YUlmH0?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>

2. Enter the following information.

\


| Field     | Description                            |
| --------- | -------------------------------------- |
| Host      | Hostname of the database.              |
| Port      | Port of the database.                  |
| User Name | Username to access the database.       |
| Password  | Password associated with the username. |
| Database  | Name of the database.                  |



3. Click the "Test Connection" button to validate the connection. Once a note is shown ‘Connection Successful’ in minutes , you can click to Connect to continue.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfq24Cxvr7--mFERa9eoq6YiT5JBnZep9zKtrAnX-1LZSCpPqXD7OVTO3-C2baffKmN8XsapTP2yahg1M0DdRLIWC_xlKHCYudw2elM5_rAwROdKMzpzTL_9CUQJqlBQaz5zcLWEUS2Bd39o4UU4HU0A6KU?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBDo8Tu0MZITPFII3pzAPY7kVxqaGRdh8uioGpX-FoUmKAr9ot2tWaSic83JgxzV2DzI-gmxpNsUk1UwEnKTGwwskyciUR8Tsclw4YGMaL6gMy8cHhQWdEsdhrgPcvtsKR8yRI4RTExmr2iTUuwQX46gc?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>

4. Choose the sync frequency required for your business.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdbVi9APOJss5GVx9B09yUi3ytlYZIOR1_26KDmzk8J9qq7anHmu1hn_vr0D7r_S3PJBeXdtYsxu7m5lZWc7t4mv0WxW2iRP2I2_qCewr2ipQJansjaAZdgE3Vkh4fMldeQQDzcztlVfk_6QB1tq0WThAIU?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>



5. Click the Add button to add table(s) needed to be synced from the database. Select ‘Incremental’ or ‘Full Refresh’ as the Sync mode.&#x20;



If ‘Incremental’ sync mode is selected, your data will be only replicated with new or modified data, where additional information is needed: the cursor field and primary key.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcyJ22OoGgj3CoirbphzHFQDuJX_PQgxlUJ2Jo-6AMnBsiogE9--7qHLs9FExtLgvz-RYlNKdEtIoNbv78tTwOO4NFG3cPWXCjOPLTCkya6x9-FGc2ColMPl28tRT0BFQNIhzuOIz6bmEk9Lo6jGyoxsWY?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeSbh8i_5fVECZzpmwBHVVFhBqGz2UKBW7OjbXZ3z3d_rHPA82brbKDLIJLJJHi5P4JVt8Xx_MTsF3UOHhBsxns-zMPl4Qgf5gHaNZqYyLIZ426jdvobvYv2pTdQqBPJF6ZJmgqX9dA9htsFibnCALIe18?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>



If  ‘Full Refresh’ sync mode is selected, your data is retrieved from the source, regardless of whether it has been synced before.\
\


6. Click "Create Connection" to start the data synchronization process.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe_FHY5Lqib-Gqu9Pa4M8LJmfRnxX-f2JCLSvGWDhmxJxnQ9Lv-NoTNvjt9n2rwF0rm0uug_7xaHmhSqr8072lMzx2nOwvf-U3TWy5-_s2WcgimPXPNgYxKGNlLJQU3ZYj06iWvWKLBP2j3tt82OQ_nH6Y?key=jGwPjPGalS5OzQ3O6o42BA" alt=""><figcaption></figcaption></figure>

