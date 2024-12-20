# Label Tables

Here in Telegraph Bay, we offer tables of labels. Users can find these label tables listed under the 'Standard Table Categories' section of the menu.



1. **`project_address_label`**

* The table contains labels for **users who have linked their wallets** to a specific project.&#x20;
* These labels include, but are not limited to MPC, AA, Custodial, and others, which are provided during the on boarding process.
* Please note that each address may be associated with multiple labels.
* This table can be used for such scenario:
  * Blacklist excluded
  * Address selection
* You can check [this chart](https://www.telegraphbay.app/chart/project-address-label-fp-44813) to see the address stats of this table.

| Field       | Type      | Description                          |
| ----------- | --------- | ------------------------------------ |
| address     | string    | Wallet address linked to the Project |
| project     | string    | Project name                         |
| label       | string    | Label of the Wallet Address          |
| blacklist   | boolean   | Is blacklisted from Project          |
| updated\_at | timestamp | Label update time                    |
| created\_at | timestamp | Label create time                    |

2. **`project_operation_address_label`**

* The table contains labels for **operational wallet address** provided by the project team
* These labels include, but are not limited to Locked, Unlocked, Treasury, Reserve, Advisors, etc.
* Please note that each address may be associated with multiple labels.
* This table can be used for such scenario:
  * Unlock and Holding Status in Token City Map
* You can check [this chart](https://www.telegraphbay.app/chart/project-operation-address-label-fp-44812) to see the address stats of this table.

| Field       | Type      | Description                                             |
| ----------- | --------- | ------------------------------------------------------- |
| address     | string    | Operational wallet address provided by the project team |
| project     | string    | Project name                                            |
| label       | string    | Label of the Operation wallet address                   |
| public      | boolean   | Is pubic to All Users                                   |
| updated\_at | timestamp | Label update time                                       |
| created\_at | timestamp | Label create time                                       |

3. **`project_nft_operation_address_label`**

* Similar to `project_operation_address_label`, address in this table are **operational wallet address** provided by the project team.
* Differently this table provides more information of which collection and what business type  (i.e primary sales or royalty)  since address in this table are designed for the purpose of receiving revenue as NFT primary sales and royalty sales.&#x20;
* These label are already included in `project_operation_address_label`&#x20;

<table><thead><tr><th width="262">Field</th><th width="121">Type</th><th>Description</th></tr></thead><tbody><tr><td>collection_name</td><td>string</td><td>Name of the NFT Collection</td></tr><tr><td>collection_contract_address</td><td>string</td><td>Contract Address of the NFT Collection</td></tr><tr><td>project</td><td>string</td><td>Project name</td></tr><tr><td>wallet_address</td><td>string</td><td>Wallet address</td></tr><tr><td>chain</td><td>string</td><td>Chain of the Wallet Address</td></tr><tr><td>business_type</td><td>string</td><td>enum: Primary_sales, Royalty</td></tr><tr><td>solution</td><td>string</td><td>enum: token_transfers</td></tr><tr><td>updated_at</td><td>timestamp</td><td>Label update time</td></tr><tr><td>created_at</td><td>timestamp</td><td>Label create time</td></tr></tbody></table>

4. **`contract_address_label`**

* The table contains labels for **contract address** gathered from external source
* These labels include DEX and CEX for now and more will be added.
* Please note that each address may be associated with multiple labels.
* This table can be used for such scenario:
  * Token inflow and outflow
* You can check [this chart](https://www.telegraphbay.app/chart/contract-address-label-fp-44811) to see the address stats of this table.

| Field       | Type      | Description                                             |
| ----------- | --------- | ------------------------------------------------------- |
| address     | string    | Operational wallet address provided by the project team |
| project     | string    | Project name                                            |
| label       | string    | Label of the Operation wallet address                   |
| public      | boolean   | Is pubic to All Users                                   |
| updated\_at | timestamp | Label update time                                       |
| created\_at | timestamp | Label create time                                       |

5. **`ecosystem_address_labels`**

* The table contains labels for all Animoca Brands Users ， which means:
  * Holders of Animoca Brands On-boarded Tokens and NFT
  * Linked Wallets of Animoca Brand On boarded Projects&#x20;
* These label are generated from `ecosystem_address_features` , include&#x20;
  * Wealth Level labels: NFT\_millionaire, Token\_millionaire,etc
  * Engagement labels: last\_30day\_active\_on\_web3, AB\_web3\_user, etc
* Please note that each address may be associated with multiple labels.
* You can check [this chart](https://www.telegraphbay.app/chart/ecosystem-address-label-fp-44814) to see the address stats of this table.

<table><thead><tr><th width="168">Field</th><th width="126">Type</th><th>Description</th></tr></thead><tbody><tr><td>address</td><td>string</td><td>AB user wallet address</td></tr><tr><td>label</td><td>string</td><td>Label generated from address_features</td></tr><tr><td>source</td><td>boolean</td><td>Source of this label, currently  <em>system</em></td></tr><tr><td>updated_at</td><td>timestamp</td><td>Label update time</td></tr><tr><td>created_at</td><td>timestamp</td><td>Label create time</td></tr></tbody></table>

6. **`label_type_setting`**

* This table is designed for label classification.
* Users can find the above labels and its related type in this table.
* You can refer to this [chart ](https://www.telegraphbay.app/chart/label-type-setting-fp-44815)to see what label types are now in this table.

<table><thead><tr><th width="168">Field</th><th width="126">Type</th><th>Description</th></tr></thead><tbody><tr><td>label</td><td>string</td><td>labels in Telegraph Bay</td></tr><tr><td>label_type</td><td>string</td><td>label types in Telegraph Bay</td></tr><tr><td>updated_at</td><td>timestamp</td><td>update time</td></tr><tr><td>created_at</td><td>timestamp</td><td>create time</td></tr></tbody></table>



