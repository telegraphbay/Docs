# Airbyte API Connector

1. **Setup**&#x20;

Click on builder -> start from scratch

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdgzH6ihaM1w7TgfXIUWpqDvKTV3P7OQ3xhY_EEdvDJupQQiJeW9lYRDkKPql1jLguuDP4BhMX9y0Zqx6yT9c6brAwVKDh6-uST2AMCk8lUYpARIcnH0q8uAaf1Emt1akieC59Z_OU_trTnxplHb1gyJxY?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

2.  **Setting up global configuration**\
    \
    On the "global configuration" page, general settings applying to all streams are configured - the base URL that requests are sent to, as well as configuration for how to authenticate with the API server. \
    \
    You can find more information about authentication methods on the [authentication concept page](https://docs.airbyte.com/connector-development/connector-builder-ui/authentication).\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeWmx76iKfbu0IMoBGGPsZU4Mtn_P9kheMz93r-rUaxryJZGtYocUn9be_Exrfgu-CUa_ZHwmLn9p2P-JF6c8nKfeuktYM7zFFJCDCBzXtd6LrmYo_tLLRk8dzNjascfWzcm-Wk-HvJedYWOAzoXiORRpbj?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
3. **Setting up and testing a stream**\
   \
   Set the Stream name and URL path (like: /v1/{api\_name})

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZLxWcN_8idbrPUFdtoH5qTbYVo-uLdWia4lElDgiYiLzGwKRrRgbU1XqwwyyKrRPH_v8qWQkdZeVDKfrzg7he591kB5kLLXZYyWAn-QjW62oFxMYQ1JQgXmb0xHzlEtQovZJcGNT8Jlle4JVQn2PhWlM?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

\
Set api-key and require params, now the basic stream is configured and can be tested.\
\
To send test requests, supply testing values by clicking the "Testing values" button in top right and pasting your API key in the input.\


<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXe50-W-hCNGsxf9vmkfNVWZtOi7rawa-pXQiqG1_4JDoiUvJUL0cJlP6oPauH8hxG27ISGw9Y-9soHqvf2RwE2i9rtcxDgmN8erLLTcZTlZB8PajIknkNVq_6L9UZI-WNzrOc_7AwOdquFfcIgSakJHnM8O?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

Then you will get the result of this request. Click on 'Automatically import detected schema' to automatically import the schema of the result.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfA3aekXz0D0zqy5g4_hwKC5enJB7jYJc9Q0kPxB4AO2w4NICapwNEvclgi_GhixfknyAQiEChXrtuwUyiQtB0OzNu4VwdA4mjh62Sg300JDl5SC9VGS7a9sNpTBaQRoQyk-Aiidp5D1VGAaNd4IrvAE3sj?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

4. **Advanced configuration (Optional)**

*   Record Selector List of potentially nested fields describing the full path of the field to extract. Use "\*" to extract all values from an array. See more info in the [docs](https://docs.airbyte.com/connector-development/config-based/understanding-the-yaml-file/record-selector).\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdE5VWVJtLP5hFKDvtAeru9WthaFma-MFXZZHxzVXL6A3c_QUf-mwW73_VEcN10h_g_eU6swNthIMNX95oMgkazGzJJSc5cuOgRL8I3xktMh-K0djS9DVd6tdOaaEzpEC4IKk-SZyJqFzl5FsCZbLnUvju1?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
* Primary Key\
  The field to be used to distinguish unique records. Can either be a single field or a list of fields representing a composite key.
*   Incremental Sync\
    you can configure the incremental key, if api have incremental key option\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdnIy70ow6sDR5zuaK1GPYYpottyOJvEajRhjOm4OFe8q_a04ZRCHdFo__mbqtrs-2PkXYUURnWYN048sQ5fBg3g_FX1NYpKwu0mQvJu3rOkwQfoRLMQtuq7708SNxv2MooauPKtZIJWqKvSRB2Q3j_1RjL?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
*   Transformations\
    A list of transformations to be applied to each output record.\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdv_rXTvtf0tTyWshVi-jiB89pjL2v8DnC6OcdPNjkF83od2A0_5Q48ExvJW1Sc8w31dXZ4Q9mgkzcFc0Xug4TwY8uQlWFa9gPptGssuRkfO2zT7Kt1KftQvQkxXed7wo7i_JCZYl93kU2Kc-FvR7Q76ac?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

5.  **Publish to workspace**\
    Click on 'Publish to workspace' to publish\
    \


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdrPEkDOaQilIpAF6Bflas79oFPi6zkkQKrp-Fwu4iH8pFq8eWyNgpQ0WpJv-bHQLlC4i0699I0fVlmQWKqwbbhCFwESxIglQR8qJBMw-Vc95cMWq5AgzcuQrjz3-raiV-DXFf7WqcMZBzor9g8tzSkmJY?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>


6.  **Create source**\
    Sources -> New Source -> Search API -> Config and set up -> Test connect\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfUn6BGG1nv2r4FH-hEJw9Y6eWf1gyB4t0a-XMfUwJ7ZNf6AXDiL0z0oRfkIDgRFqAE6rOBzQNWEsN3vo5MojtBEXnbMVq44_beXWrsF_R1WGrknzdZF_PqHynrTnI2nDkKonjtQ27a_v6wVzvi31mrEIEn?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>



    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXec0ngBfZq0krVdvZHjZzGdTAKYOlQb_zpTY3ZgsYeYmksAV0OvvFVaBmb-J_Yfg96uOCUBCO6Gm9oVMENmqrzuJ1-MHwaliC8cgbVcSchFxVYJ1_2gfi_OeTUJ6JTYCIABLtm-mAv9Tz5e9b59_TMpk9E?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
7.  **Create connection**\
    Choose destination -> Test connect -> Config connection\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcsHz9oJFrE6_NT2qPW58d49l5z97ZHhEKk638Dq4yPKkkGoz50xNO2aE90RmNJbruvVSaNjE9_laM-ygOlssG-RdtsW95gDn6DEkeb8giZuFhA2ibmdS9usYlx_JvhYgAGz9L-fYUFhMFoBKTEe1-hgM8Y?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

*   Replication frequency\
    Set how often data should sync to the destination\


    <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeE-9Dm8bhyQXpcpBuCoaBjT-NcqC_T1Uva8M1osvPzB8bCH7RfNO4vrm15gH5Q2j0hySXSVjC_dNvIxbs6nywsU-YtO9SR8numxDiRemnZtuUHNiFJ6sDot6ukF0was8s1nxLrspGVGaBDDK-E-7lVeiU?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
* Destination Namespace
  * Define the location where the data will be stored in the destination
  *   Table path: "connector-data".\{{your schema name\}}.\{{table\_name\}}\


      <figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfaB67V9w2T8kA8utjQdqV2afBLFDmDUwqO0a5-1Ic1ElAqqIpjg2_qkiMEwPTVcSJ530D5aCgg7PQZihcGBW_f58YMidvpQh165jtQIX0v33kAlDDsVl4kGU_zgj3-7ouem5ZTWTCELyTrFjsGQ-x5nJo?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>
* Destination Stream Prefix (Optional)
* Detect and propagate schema changes (default: Ignore)
* Choose tables

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXeRRr4YsOP_4pTxYMWQn0wd5xZv-8g1jHWptZuitX9YHpxa2P0PyqphAxHIBl_CCE_sr7SbUKaXTI4b-ElQd_4xJLDxsLLBQ1eewxzfVee7SikstPHmyNWrmUe9e0B9jczTM1Kss6_ptVwGjl8R3MdfcreS?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

* Click on 'Setup connection' in the bottom right corner\


8. **Sync Data**

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcBsBrthq8JUJ-LNcKJcccTXhUl83hlJQ7kL-frk1F0hdtDW-ar0tlDiW5oPa3gX2bLAMga--SFG4lmyIie9qvRpMyQB4HwoI6O9inxiGEs5TdFGoAMUeercHZ3CuJz9uw7UEqzMSm5sH6h-Ot93JVuF_o?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

You can view the synchronization status in the ‘Job History

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfiWy-He_3N0ec7DYsq-O_Upy2cBMBoaGAY6BMk3ua1PVmc24AuJZMGahnhFOEACfJ0XsYq_jCxa_NDerMN8a1aPfKnkQIE6UBpbIkUDVs1PIc5vzz5YlZc_83UJ8aEsWwuvIrcCKBYAy3nb5yTgJB-rC_V?key=AfL3CTZ2-d1l11LxxHze8w" alt=""><figcaption></figcaption></figure>

Finally, you can obtain it through an SQL query like "connector-data".`your_schema_name.table_name`, for example: `select * from "connector-data".public_test.nft_collection_stats`
