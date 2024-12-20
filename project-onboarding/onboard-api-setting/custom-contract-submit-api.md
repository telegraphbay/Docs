# Custom Contract Submit API

1. ### The main function of this API

The contract submit API is a tool for internal users to upload essential information about contracts that require event decoding (such as: chain, contract\_address, abi, protocol\_slug, etc.). This API enables the submission of contracts and triggers the automated event generation workflow.

2. ### The Design of the API
   1.  ### table design 

       <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeGA1sx0it66d1b9yG113865TURqiB6NUMrTtFOt1zQ1fakhzK64K1ahadYEjKM7ULOJb_kmNSEJ5tNqQGFufrOH_V32cXZbkjPGUUsp6Ci7I6Gh6BYtLXyg96mj81h56GC-i3TDmx0I6n6cE_VD9nUjCkq?key=KFXCvjkP30Qk23QeCPEVag" alt=""><figcaption></figcaption></figure>
   2.  ### Workflow   

       <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcB-dZk1q4YRtcyaLZCqTifNhjbsXui5w4eMwlX_h3Fv9pFdRolpfTggIAppgotEQ-kviAJSdt3qGXmX1vP8f2nI8dYxFbLwNHL38ifsXwXL4WJ0JcCmwcBX7o2ioeYGZI1sJ9_naasQEDK2lmWWDSNNSk?key=KFXCvjkP30Qk23QeCPEVag" alt=""><figcaption></figcaption></figure>

       1. Internal users can submit contracts that require event parsing and ABI information through the API.&#x20;
       2. During this process, the API will verify if the ABI format is correct, check for existing data in the database, determine if it is a new version of the ABI, and based on these checks, store the data and trigger the airflow event decoding production dag.
       3. When entering the event decoding production process, the system will parse the events of the contract based on the submitted contract\_abi\_info and {chain}\_logs table.&#x20;
       4. Contracts submitted to contract\_abi\_info will undergo incremental event parsing on a daily basis moving forward.\
          \

3. API Call
   1. URL  \[ POST ]
      1. staging：[https://www.staging.telegraphbay.app/api/v1/contract/submit](https://www.staging.telegraphbay.app/api/v1/contract/submit)
      2. prod: [https://www.telegraphbay.app/api/v1/contract/submit](https://www.telegraphbay.app/api/v1/contract/submit) (to be deploy)
   2. Headers
   3. x-token: Requesting necessary authentication token for the API &#x20;
4. Parameters (JSON)
   1. chain: Chain of the Contract
   2. contract\_address: Contract address
   3. contract\_name: Name of contract
   4. protocol\_slug: Which protocol does this contract belong to
   5. operator: The operator updating this data is only for recording purposes
   6. abi: Abi of contract,used for Event parsing\
      **Usually, you can find the ABI information in the scan as shown in the image below. Just click the copy button to paste it.**\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdWx-N7sHSh4gDih6SI8QAuOgM9RRgpeMH3RWbEj0g2d6XsNx_gYTKFu48YqEtkdnabEJ3FnaY6d2f7_bxq8jPIQyF4OI2xH2AszrKgBIXrPjNeE5SbWtZuxeSa46W8oZVJLsMpC6JA6JHJ2gRNsaCSQDQ?key=KFXCvjkP30Qk23QeCPEVag" alt=""><figcaption></figcaption></figure>

4. Example

```json
--header 'Content-Type: application/json' \
--data '{
   "chain": "Ethereum",
   "contract_address": "0x9a98e6b60784634ae273f2fb84519c7f1885aed2",
   "contract_name": "SimpleStaking",
   "protocol_slug": "mocaverse",
   "abi": [{"inputs":[{"internalType":"address","name":"mocaToken","type":"address"},{"internalType":"uint256","name":"startTime_","type":"uint256"},{"internalType":"address","name":"owner","type":"address"},{"internalType":"address","name":"updater","type":"address"}],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[{"internalType":"address","name":"target","type":"address"}],"name":"AddressEmptyCode","type":"error"},{"inputs":[{"internalType":"address","name":"account","type":"address"}],"name":"AddressInsufficientBalance","type":"error"},{"inputs":[],"name":"EnforcedPause","type":"error"},{"inputs":[],"name":"ExpectedPause","type":"error"},{"inputs":[],"name":"FailedInnerCall","type":"error"},{"inputs":[{"internalType":"address","name":"owner","type":"address"}],"name":"OwnableInvalidOwner","type":"error"},{"inputs":[{"internalType":"address","name":"account","type":"address"}],"name":"OwnableUnauthorizedAccount","type":"error"},{"inputs":[{"internalType":"address","name":"token","type":"address"}],"name":"SafeERC20FailedOperation","type":"error"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"previousOwner","type":"address"},{"indexed":true,"internalType":"address","name":"newOwner","type":"address"}],"name":"OwnershipTransferStarted","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"previousOwner","type":"address"},{"indexed":true,"internalType":"address","name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"internalType":"address","name":"account","type":"address"}],"name":"Paused","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"user","type":"address"},{"indexed":false,"internalType":"uint256","name":"amount","type":"uint256"}],"name":"Staked","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address[]","name":"users","type":"address[]"},{"indexed":true,"internalType":"uint256[]","name":"amounts","type":"uint256[]"}],"name":"StakedBehalf","type":"event"},{"anonymous":false,"inputs":[{"indexed":false,"internalType":"address","name":"account","type":"address"}],"name":"Unpaused","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"user","type":"address"},{"indexed":false,"internalType":"uint256","name":"amount","type":"uint256"}],"name":"Unstaked","type":"event"},{"inputs":[],"name":"acceptOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"newUpdater","type":"address"}],"name":"changeUpdater","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"getMocaToken","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getPoolCumulativeWeight","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getPoolLastUpdateTimestamp","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getStartTime","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getTotalCumulativeWeight","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getTotalStaked","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"getUpdater","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"getUser","outputs":[{"components":[{"internalType":"uint256","name":"amount","type":"uint256"},{"internalType":"uint256","name":"cumulativeWeight","type":"uint256"},{"internalType":"uint256","name":"lastUpdateTimestamp","type":"uint256"}],"internalType":"struct SimpleStaking.Data","name":"","type":"tuple"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"address","name":"user","type":"address"}],"name":"getUserCumulativeWeight","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"owner","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"pause","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"paused","outputs":[{"internalType":"bool","name":"","type":"bool"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"pendingOwner","outputs":[{"internalType":"address","name":"","type":"address"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"renounceOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"stake","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address[]","name":"users","type":"address[]"},{"internalType":"uint256[]","name":"amounts","type":"uint256[]"}],"name":"stakeBehalf","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"address","name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"unpause","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"unstake","outputs":[],"stateMutability":"nonpayable","type":"function"}],
   "operator": "alex"
}'
```

4. ### Check task progress on Airflow
   1. [Airflow Dag\
      ](https://airflow.staging.telegraphbay.app/tree?dag_id=evm_chain_decoded_events_history_trigger)In this Airflow DAG, you can see the event decode task for the most recent contract submission.\
      ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc_uMQTX9cij2hssJYNiUbF4ovlUEx1CWOfFZaK2HO4NQ419NB0b3dr6ubUZvE3042l5rQWmh7Tzlh0CqfeNDmeufDS9NDgSC13Zx5nQK2P6bS8bNsNiws-idieGmQP77s1FAFBO7VsD4FlrPqMmTbrl-q8?key=KFXCvjkP30Qk23QeCPEVag)
5. ### Query On TB

Once the event decode task is completed, you can run a query on TB.

4. Decode Event table

<pre class="language-sql"><code class="lang-sql"><strong>select * from iceberg.animoca.ethereum_decoded_events
</strong><strong>select * from iceberg.animoca.bsc_decoded_events
</strong>select * from iceberg.animoca.polygon_decoded_events

</code></pre>

2. [example\
   ](https://www.staging.telegraphbay.app/@Bond/Moca-decoded-event-dashboard)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeMERvpRAJmv91JBA5clm-QJ9PvF4yUQa8M8E8S0l10AZarjIj4dGR3tJKp3FawE8jgshbwGKnvdtdFiVAO-Z57ij8iMknfRxeJ4oEjIzYtB1NRuBWuEH8lKsKy9zXya2r1sP0BmW6ratvmOLXjMIRbN8qS?key=KFXCvjkP30Qk23QeCPEVag" alt=""><figcaption></figcaption></figure>

\
\


\
