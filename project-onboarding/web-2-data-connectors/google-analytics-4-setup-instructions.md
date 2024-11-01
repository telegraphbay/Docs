# Google Analytics 4 Setup Instructions

## Step 1 Create a Service Account for authentication

1. Sign in to the Google Account you are using for Google Analytics as an admin.
2. Go to the[ Service Accounts](https://console.developers.google.com/iam-admin/serviceaccounts) page in the Google Developers console.
3. Select the project you want to use (or create a new one) and click "Continue."
4. Click "+ Create Service Account" at the top of the page.
5. Enter a name for the service account, and optionally, a description. Click "Create" and "Continue."
6. Choose the role for the service account. We recommend the Viewer role (Read & Analyze permissions). Click "Continue."
7. Select your new service account from the list, and open the Keys tab. Click "Add Key" > "Create New Key."
8. Select JSON as the Key type. This will generate and download the JSON key file that you'll use for authentication. Click "Continue."



Note that the instructions provided in the sources may vary slightly depending on the specific Google service you are using. Be sure to follow the instructions that are relevant to your use case.



Service account key file should have the following format:

```json
{
 "type": "", 
 "project_id": "",
 "private_key_id": "", 
 "private_key": "", 
 "client_email": "", 
 "client_id": "", 
 "auth_uri": "", 
 "token_uri": "", 
 "auth_provider_x509_cert_url": "", 
 "client_x509_cert_url": "" 
}
```



## Step 2 Enable the Google Analytics APIs​

Before you can use the service account to access Google Analytics data, you need to enable the required APIs:



1. Go to the [Google Analytics Reporting API dashboard](https://console.developers.google.com/apis/api/analyticsreporting.googleapis.com/overview). Make sure you have selected the associated project for your service account, and enable the API. You can also set quotas and check usage.
2. Go to the [Google Analytics Data API dashboard](https://console.developers.google.com/apis/api/analyticsdata.googleapis.com/overview) (If you are using Universal Analytics, please go to the [ Google Analytics API dashboard](https://console.developers.google.com/apis/api/analytics.googleapis.com/overview)). Make sure you have selected the associated project for your service account, and enable the API.
3. Go to [Google Analytics](https://analytics.google.com/analytics/web), find the properties & apps, and add the service account as a viewer in account access management.



Note that the specific instructions for enabling APIs may vary depending on the Google service you are using. Be sure to follow the instructions that are relevant to your use case.



## Step 3 Complete Configuration in GA Tool



1. Select "Google Analytics 4" as the source type.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeLdWeIjsqkZsXR5NV7J-WufeNws95DlICNMcZWrIE4Ew8UHWwV478wxNbkWwUzHbJmKogmrIcvFKqa5m5xVqzQf2XmvrrUuW9TWGkkN7BrEINubT8KmWSJAPQlb-lMX7EOwijBm6XFZHZhYssxtpjLheY8?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

2. Enter the Property ID whose events are tracked. This ID should be a numeric value, such as 123456789. If you are unsure where to find this value, refer to [Google's documentation](https://developers.google.com/analytics/devguides/reporting/data/v1/property-id#what\_is\_my\_property\_id).\
   \

3. Enter the Service Account Key that you found in Step 1.\
   \

4. In the Start Date field, use the provided datepicker in the format YYYY-MM-DD. From this day onwards, all data will be synchronized.\
   \

5. Click the "Test Connection" button to validate the connection. Once a note is shown ‘Connection Successful’ in minutes , you can click to Connect to continue.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcVzbUQXXFIXVLVFhiVUKNHPsn5NhrJFlwhQ1cglSmp2ypq11Hwq3oxITjxiDSbwMG_YDCps2bUnXKj4hIyUqSkh-_InHw-OGXLAQOlTe6Cp5fWwc-V5zZg0CKfN9jAOZFBPnCv2Ati54KxpElM1JQLoyzw?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdBQ38ACo2legAu9VUjmckGvpLlW_EWRixs0C3JC508-iHE9kfCL33BsA0C9G7gdwdj_UzWlzzFtz5o6TdGFyKaSKapbLl6czcKuSd9bUGxNjaFvGoA4nZqNXK44Q8ZLHbBVaxoQA6UCfUVKibGR_WiqDQ?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

6. Select the frequency of data synchronization required for your business.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf6y6lOdV4MKtxpxLuYsSUoS68YMbniYAc3Q7QictgXmfEEHVPN53tBWKYaLUhCvfHRJI1O9hqvKhu_BSzZIvujevtax85v-otGXLBR_TwqdL948xU7MQbfGNN9DHARW1m59-sOvUkBM1uV1WSEPiP5QFZb?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

7. Click the Add button to add table(s) needed to be synced from the database. Select ‘Incremental’ or ‘Full Refresh’ as the Sync mode.&#x20;



If ‘Incremental’ sync mode is selected, your data will be only replicated with new or modified data, where additional information is needed: the cursor field and primary key.

\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfyU8SUDKjAldzgEXpr1_o-n82eLmqOoLPspe778leGP9HDEgCrfxa52prEHCJ__QSRfrvqwKMIYEjRWjY0uR6d6UzcMaF8mHuaqRSlTJDGGROUAZSGFskbMNQIii4OTNBp023QaPYRNDDngfDP8kdbzjaQ?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe8zwsS2hnc5WXOAG275tghcUUkiD-_LE8tzdJh1XmlrIZ09S2YemruFP_Bhw4wB0SH3NtIq0wbMHpcLTrVRJxtTDAdd0Ndk4UmhBEFbJ5BE0KY6qx-vzOv82ip0JjZ7xzLvkDIicA6TlgJXxNtn2IWAk0?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

If  ‘Full Refresh’ sync mode is selected, your data is retrieved from the source, regardless of whether it has been synced before.\
\


8. Click "Create Connection" to start the data synchronization process.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXet0UE9FZFUtsmuV8AhCV6xXkJKTJYCCznPZq63vW7CP7FXamGrw6TZZRPoB7Je8V80o-5cIMuu3Jibn8PSQHi89qQMyqsHSIzfnGDxtwaj05nRZMGlkWZNCiR6zu5Oq3DAiHtBFYQffORh8uMXcHiBFpf7?key=2E8woSGkM1hQmBbOohds5w" alt=""><figcaption></figcaption></figure>

\
\
