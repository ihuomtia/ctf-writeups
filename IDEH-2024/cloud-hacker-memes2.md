This challenge is akin to `hacker-memes` and features a static web page with a broken image link:
![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/hm2.png?raw=true)

Upon inspecting the source code, we observe that it uses the same link as the first challenge, but now with an additional comment `<!-- need to move the memes to the 2nd storage area and repo lowlevel01/hacker-memes -->`:
```html
<html>

<head><script type="text/javascript" src="https://tiiny.host/ad-script.js"></script><script defer data-domain="hacker-memes2.tiiny.site" src="https://analytics.tiiny.site/js/plausible.js"></script></head>

<body>
<h1 color="red">hacker memes</h1>
<img src="https://ffd6da3792c95a653c6b8c8b47c7e65f16c3c28adeccec6fbb7704d-apidata.googleusercontent.com/download/storage/v1/b/ideh-website-storage1/o/01koR8gU0hz7diuCyRnS1xH-3..v1569489628.png?jk=AanfhSA-BYWYRRsfnmsaHeMNY4aZjN_2fprd5ei1HGLxGPKvptAdyUqp-zvUm8S2Y8tb_14HKkXd5PYuFNIvSE0ZVK4pTyb7bSNJAgS91_qowkYvItN_9v7fUwYCWquv4c9SbeuUWsI6DC0frV_KmX6BcRjsfbbbc9PzHRlo-31Dm_ClR6iCsI2RBBc-MqRhyp9VaxL3YlAszIVDSUiu6eawC-bnJ7OhNA149QXpBcv0d1OiU7j1A6bB9qrHkjhK7r-fYT-akmkDvoOsFA3Nt9MlEcoF_YvHtKW-0M7K4vC3hB2M_nt-YzwnrGtlXdGY8D2jbYt4YuQvxArMyOe8T_5PZI65LJBcPyxZ0mWbI24tWLqyii31-wUf8V79ErWPICIaEgmD6TBUmyjtM7NOfZEJlj9bdO8Zfub3eBYozknn8-Z7M8esVHsYetvqjj5LtyTB-jBPSXydY8knMUw0ZFySxQ1yIWtWlcZ0jJGvfqIPdrDaZIMp1JKT4WsWMyoYk5rWHM18EXl6jY-B3PM4DjeCLauZ9E5lcFepK6vC8vtHiWLJAfe_BqyVrvJhmC2f98IJG8YUCs8L01mhOHvy-9jn9fXrJzSYD2JQOTRGRQHYA1hS0cchbfuywEhw-QGluMVoPDkkgzmnm5cn80HEoPV377sj0wATLudte_zPGUgyxm2dsBOqbxgtZaFiY7j-fBDqz-qEPVAiuupHnGuxRJn47KXmeEdfoqodyqoBtvU5YMnJkgiebSzgiZfpNHX0gGaugNr3ryRVszTRHvGYE8wzUHvgLvn-LBLNlLDgyicrddNNswu0syPTIyntK-PYZGFrNeeffoZDZCv8aCx2AIQOhCyATPmAois-pcawKj14tKBcaSjkmYzuUW7yEt0x4rYR4RUsufD6NBbQYZmALfDuXiHwA0KfrQRhQcVi4ugomRS7pa7eiw6rWFY-7Ttjaj2K5k95T_nrWZbXormMtDq-lhScKYqAbcvHyAqc3nfkGstMy2p-zmaNiaiSq5m95bi28cUje89FqT9L6zFXQ4-l4SID_alQmkXYeSetzk61gu8bt35z-c2j3gbrrEap0FXRhIlVNL-vaZ4yL0S5zHDrAxZphxeg74NqUaQE7H2f62atF5tH88QAu4SUaS0zKsqozFv6aZUA16GHgl73sQyr3YHUBMwbvGg&isca=1" />
</body>
<!-- need to move the memes to the 2nd storage area and repo lowlevel01/hacker-memes -->
</html>
```
To investigate further, I checked if `lowlevel01/hacker-memes` is a GitHub repository and found a repository containing a single file `index.html`:

![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/gh-repo.png?raw=true)
I then checked the commits:
![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/commits0.png?raw=true)
Upon inspecting the commits, I discovered what seemed to be credentials in the first commit. Further investigation revealed that these are [HMAC Keys](https://cloud.google.com/storage/docs/authentication/hmackeys).
![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/commits2.png?raw=true)
To authenticate using `gsutil`, I executed the following command:
```bash
~/$ gsutil config -a
```
Which then I typed in the credentials:
```
This command will configure HMAC credentials, but gsutil will use
OAuth2 credentials from the Cloud SDK by default. To make sure the
HMAC credentials are used, run: "gcloud config set
pass_credentials_to_gsutil false".

Backing up existing config file "/home/ihuomtia/.boto" to "/home/ihuomtia/.boto.bak"...
This command will create a boto config file at /home/ihuomtia/.boto
containing your credentials, based on your responses to the following
questions.
What is your google access key ID? GOOG1EZHALL53KQH4DPW5YZGFZTY4LOH63ZWAGHJM3ZLXOCFMAJXGPLVRKU5X
What is your google secret access key? TQQm49BT/NxZUQFXTp/Kbmco12d2cxWWRvKYSmFQ

Boto config file "/home/ihuomtia/.boto" created. If you need to use a
proxy to access the Internet please see the instructions in that file.
```

Subsequently, I disabled pass_credentials using the following command due to interference with my gcloud account:
```bash
~/$ gcloud config set pass_credentials_to_gsutil false
```

The comment in the website mentioned a need to move the memes to the 2nd storage. Attempting to list the second storage resulted in an access denied error.
```bash
~/$ gsutil ls gs://ideh-website-storage2
AccessDeniedException: 403 AccessDenied
<?xml version='1.0' encoding='UTF-8'?><Error><Code>AccessDenied</Code><Message>Access denied.</Message><Details>user1-484@propane-girder-410817.iam.gserviceaccount.com
does not have storage.objects.list access to the Google Cloud Storage bucket. Permission 'storage.objects.list' denied on resource (or it may not exist).</Details></E
rror>
```
Checking other commits, I found a phrase indicating there's only a README file in the bucket.
![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/commits1.png?raw=true)
Reading the README file using `gsutil cat` revealed the flag:
```bash
~/$ gsutil cat gs://ideh-website-storage2/README
Congratulations for solving this challenge!
One should always be careful about hardcoding confidential stuff. This challenge is a simple version of a real life issue many developers fall in.
You deserve to have the flag

IDEH{be_careful_br0ski!}
```
-------
Flag: `IDEH{be_careful_br0ski!}`
