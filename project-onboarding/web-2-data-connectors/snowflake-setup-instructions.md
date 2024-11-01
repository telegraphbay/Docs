# Snowflake Setup Instructions

1.  Select "Snowflake" as the source type.

    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfNsdYpdvXnGRqRQu5DyU9PxTehhlo-LCRgKVe4-XHiVoxgNxj94WKL71ySoaapebUaM-ADNhHUHnSTJrP5fMRWdFjBT1IEaN8HVX-9rAXuQGQTRz0mWdkeI2m2b268-ce5mHQhuUQySomXtunl0aRjW9w?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>

\


2. Enter the following information.\


| Field     | Description                                                                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Host      | The host domain of the snowflake instance (must include the account, region, cloud environment, and end with snowflakecomputing.com). Example: accountname.us-east-2.aws.snowflakecomputing.com |
| Role      | Name of the role                                                                                                                                                                                |
| User Name | The username you created in Step 2 to allow Airbyte to access the database.                                                                                                                     |
| Password  | The password associated with the username.                                                                                                                                                      |
| Warehouse | Name of the warehouse.                                                                                                                                                                          |
| Database  | Name of the database.                                                                                                                                                                           |
| Schema    | The schema whose tables this replication is targeting. If no schema is specified, all tables with permission will be presented regardless of their schema.                                      |

\
\


3. Click the "Test Connection" button to validate the connection. Once a note is shown ‘Connection Successful’ in minutes , you can click to Connect to continue.



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd-bvxuosHgvtsDoH8kGHjcJ_NR8kbsiOs68GHXkob94chopPhiEzSQBgXCVrLVDjoKWzT-yK5KEfow5sKZCua1M4bv-Uh_Syf72T4yib34U5u-fPi4V8APdYFxKVFX-N8AGpltIhdixv8AVJmbpF7dcQc?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdnGK5xh0cQqrPF6OEVSHOtA_okD46vjgw29zLvLmcTVZDYFtDxRdYO-VQbNeH3bwrjrVoLbdQKilhiVmdNv2r0LWYsELC52fUh5CFaE6ZqT0DdfXUvartijx00Z7GvUxU3dHJyC5NmRV8iDs0q3uAWlgI?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>

4. Choose the sync frequency required for your business.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdYDwH83Qp0O8EP3TeHoNPQd2yL7CUzTrLkUiqvnxv0w-4SEBtMjIcnTOHcIK10YNgpd8ZKpI0KaBNjVMZpqvKXxQklw2UKrFds61akMIyN2m4TX53swET2lldy8vqL3AP-0oNx13PpbWHFy0W6LBPNTvE?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>



5. Click the Add button to add table(s) needed to be synced from the database. Select ‘Incremental’ or ‘Full Refresh’ as the Sync mode.&#x20;

If ‘Incremental’ sync mode is selected, your data will be only replicated with new or modified data, where additional information is needed: the cursor field and primary key.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfH4s-FB8IyVBKiMMxYdwtLuLOGKyer_pQerKZksiB-KLRkD3TeTEl7w9HAV9AOm2PL30SYbm6-QaAM2Jm0A7-iP2IqPf3u3AWhJvQXuUwTy5y-Vkmo4VDc3s42jWYAHsj-qtVk7WpRsacnOdF1Tvtwr8k?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdIAiI8ImhOLVJQ-Ta1Zph3NNHEcZrQ3ABzrdMzScDxqXAmcDSqdEBZPtSf0xbIzv8ZFPDWV8AqHt7MeEBp7u2QtPOnXzl71JFsP0s27HoFdbRXDMUllHTC10yFz58X314h1UWC9JJMtGHEULduBQ-DBwOz?key=V6IPw7mHQBE0j50WtXS7Kg" alt=""><figcaption></figcaption></figure>



If  ‘Full Refresh’ sync mode is selected, your data is retrieved from the source, regardless of whether it has been synced before.\


6. Click "Create Connection" to start the data synchronization process.

\
