# PostgreSQL Setup Instructions

1.  Select "Postgres" as the source type.

    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcKE8EMfz1b9ORDFb807cVSOI78uV5NLutE8U56ENwmSt3hh7HE2bCSiwk7LtjQca0hnpdFcud3Knd1FKQDvPGMUYx4Q_ItyAaljL4xnjPyLjrYtk_WYZ3bwNQ_LXuEEJ282kN2YfXDPm51o-6IXlqmNAE?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>
2. Enter the following information.\


| Field     | Description                               |
| --------- | ----------------------------------------- |
| Host      | Hostname of the database.                 |
| Port      | Port of the database.                     |
| User Name | Username to access the database.          |
| Password  | Password associated with the username.    |
| Database  | Name of the database.                     |
| Schema    | The schema (case sensitive) to sync from. |



3. Click the "Test Connection" button to validate the connection. Once a note is shown ‘Connection Successful’ in minutes , you can click to Connect to continue.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdIP8JycqE5TT4vHP9fKEB4qyNQY8j427Pmtl1DbgsLaNoBWNL5H5Amv7sic901mRsO7EC58yOEMUMctjQa6wRaV0R_fXFqvrk2RJbIIe6zEw_7uonAAh_9CDCj_O8bpBaiVmnY96nv7U3f4hFKWxvtJsoW?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf299yuNBu6k3laScLY-Go3HdIUAIBwbd_FxSgk9Pm9uajl5pNRp4S2shzhe2gfkchQOBMm-2J5cpd4dwnnc8mwwU-0wNegiI_5z2NOWaOuHueagZnUBW2hHxxCM-Bn0bC5YVqeD1mRiMPmUyXJCZsiiu-N?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>

4. Choose the sync frequency required for your business.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfNVZTSzSxEFSJutNbf_Lab7ez7p-P2xeIwuKREEkwvp56O7Td6C0mBPblWMv_7KD_aaTiMSNcYew8Sq0crH6w7IQN9VqbVkXO2VLDVQcITLLERi68pdEhaAUQYmddku-iRc7iBUyJ6FwmtAge23nMc6ygJ?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>

5. Click the Add button to add table(s) needed to be synced from the database. Select ‘Incremental’ or ‘Full Refresh’ as the Sync mode.&#x20;



If ‘Incremental’ sync mode is selected, your data will be only replicated with new or modified data, where additional information is needed: the cursor field and primary key.



<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfWvNbvbJDab2ri9AKvC2IjGpsHP93fiwOMBVGy69ju_xEtZ6Mly7q5dwZzCIZl0iMgH5Q5MrDmyMSjLM9yZxC64vLDSl3KMDoSAkFr0bZq9aPSq-mRzLQd7aFsENQSBTY_PnNzdO9u0WQUSkg5gEZyEfER?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcX-YugJXw7z0cJArhUHXXccrEND2nvF7kv8sDf9v_HaeyzQ1ZfPyHGq2opubgSJqnYtBFSjsfzrkA7fAkPnY74LN9Q5r7_eZqL-lUpOS68vOBwokJDAddzQ_sZoyuvvFrbTzdkmnLL8iro9t5PupoiWVc1?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>



If  ‘Full Refresh’ sync mode is selected, your data is retrieved from the source, regardless of whether it has been synced before.\


6. Click "Create Connection" to start the data synchronization process.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfCtTaEnUCTMqJ9C2hS01AaHaOAKmAwlhh-qgkLswu45TsTS6_oON4AphmYefP7hZEVwzvd3zGjQ5XgwCYt-Y-pFFxQG7BO-mdTtLSlSs6J2e6-Mm2TsSgvx8TXVJsVB98JNus6ahA-p1KCb1uqkdoXwOms?key=viW_l3YpGy1N9Js5d4xaQQ" alt=""><figcaption></figcaption></figure>

\
