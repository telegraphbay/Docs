# BigQuery Setup Instructions

### Step 1 Create Service Account Key

Create a service account in the [Google Cloud console](https://console.cloud.google.com/iam-admin/serviceaccounts/create)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXddFPXDEJHSuOMvVAkGhWWeAZsWCV2BYFWYpLpYILpQqz-QfyvhzMutGzKs21rjYm_Juflf5dLbwoYjpSADx5-K81umio1xOZgvQC3CoI04sq1FZ4-1Vi8GX1UNMTa2N9dJjkZDpWqGy5F3utUkxe0FkzE?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

Enter a desired service account name and select Done. Create a private key for the [service account](https://console.cloud.google.com/apis/credentials)

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXchWlq_Fke23g2vLZjgHXZKlTmFZb7UhUuPsZ3MT9c6Dwn8FDKisXZSxANtjC4xxEilpMGbwgvk7msELKTdv-fnNj_u1bz4rtlxs94G1GlBbxco0gYmMHz1MhqT0CumFbm3W_iwh5P_aMU6ia16awkQC_c?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

\


‍ Click on the "KEYS" tab, Select Add Key and Create new key.



Select Key type JSON and click on "CREATE".

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf49CX0MN7BJ89rUMQrX81rc7tTCQxSYUayfSaCDNCflTToKz-5rFixiYgs-GtWPxc_4cIa6F_LTWn1Al-73km09mDUM-wt52XIZnvzdIsQu9PhDS2CKL21NeL3aEB6guUhqhnIIBoZgcXthhFiLmag0SZ6?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

It will download a JSON file with your private key.

\


### Step 2 Set service account permissions



Grant the service account the following permissions on the dataset:

| ROLE                 | REASON           |
| -------------------- | ---------------- |
| BigQuery Data Viewer | To view the data |

\


Note: To learn how to set permissions in BigQuery, see [Google's Introduction to IAM documentation](https://cloud.google.com/bigquery/docs/access-control#permissions\_and\_predefined\_roles).



### Step 3 Complete Configuration in GA Tools



1.  Connect based on the data type of web2, choose between users or events.\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeRaObyqNOSFZpfp9w13kjnWhX_GV8ZBmtEUn7zbA6y1Ujbgx9m7-Km7M8FC4C87Bjx6sGvfVqo27MUsf-iBaYbnrpFjSQ7dYCu6aYNPivVB-rybqcAbwRn_Un1Lj4rUpKUYoS3k0ZQrOIBH6gs0WLu5k0q?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>
2.  Select "Bigquery" as the source type.\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe16Izuz7N9rBRFbrG72quObr7XN7Og9MsSk9soKoF9eBwhWuGm2cLb6uJ2P02DupBJwhj925uEUVja04Vmd-z_sVoukOw8NvbcAqmr8gTQHlCOfgZqxTiXrcCno-LzWeuPuqiMHmsVTvBLpg0FeALc4_1M?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>


3. Enter the Project ID for your BigQuery project.
4. Enter the Service Account Key that you found in Step 1.
5. Click the "Test Connection" button to validate the connection. Wait for the test to succeed.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeefEbpDgQeqN6XMzIoUt50k_dDC7ncfF0rbAr2vf0ieDWTULJXxQgTWR4uoQ1PMywiaeTRmCpnLHawJuPjXN5C9F6-VSqpixI8aT1WSKeGhFpUTyO9DRl_8dMaH5k1KqJ0Pyk902PH_f1jtXLsfiuzNVg?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXddc7wRQzxxEoRXiDAQZ8fZ62-cvt74ab0iqoxsq5oWBzdZXskbLbOjheSTjAJZv5vqVDArsnTGFNsXaTbRuiCvE9nSMpBqiBtkyIsOWgsuFc8D4cFVNgE9Rt0zvrgVpLCFeE3G4K-A40cNHx7zJIqA8gwn?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

6. Choose the sync frequency required for your business.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdQ-7_XTzSE6r5Tz81iCHBo5LN3-xbR-XT-hFyZnSOyIUK0DsKhPW44hVlZelRS6fgSXIHUnV2EhhgPbhQ-HVrLen2MuheZUN8XQ09dx00hVB5DijrWuGnXaC88qYi_lcL4piX0Nka8SbUyI0HvHvMDyUt0?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

7. Click the Add button to add table(s) needed to be synced from the database. Select ‘Incremental’ or ‘Full Refresh’ as the Sync mode. \


If ‘Incremental’ sync mode is selected, your data will be only replicated with new or modified data, where additional information is needed: the cursor field and primary key.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeROJxjNg9ultWmwsvs_LvZEj43qPTNorBqAnLUS7WAwhbOysG2MI-1kbT9112IVBnt-LclBn9buq7krgHXTYXtX2ktAy8nPJTbayMoDSz9f3xY5bcoc7JJB1h60aqEpWOunW6uBiHxl13BbM7G0l4NtiY?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd02T4TQM5B8stXuuDQFi8sPqaUOyZ1Qzd3TR9XRrTISS0jGWRtH05iwq8GieNTgTfdVBSMIAV-kWqujIQ_CtGOUAnRGI58AFfhRHRGg6Wl_WOsLEqvpEoWmIBjSNNpJR_u1CWqHpFIR6Zan7bAGjKTy4st?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

If  ‘Full Refresh’ sync mode is selected, your data is retrieved from the source, regardless of whether it has been synced before.&#x20;

8. Click "Create Connection" to start the data synchronization process.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfoDqMyTQsNmzAaguu_IT7jSsoX2wDarCKtRv3NoOKcAb4pPV6n-AUClVX-_wJK-CJDr1DqmSowOpXY3lxdapeG1cI8gKTC6v77vJCLfb7NNgutFxvbwKvcOzjb5jPv3DQ9_dKmGfCoxDGA1rH_HJuThihR?key=5Epikl6lywq718KjgPcSrw" alt=""><figcaption></figcaption></figure>

\
\
