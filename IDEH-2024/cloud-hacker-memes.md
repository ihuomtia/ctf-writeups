## hacker memes Writeup

This Cloud challenge is straightforward: we encounter a static website displaying a text with a broken link to an embedded image.
![](https://raw.githubusercontent.com/ihuomtia/ctf-writeups/main/IDEH-2024/imgs/hm1.png)
Upon inspecting the page's source, I noticed that the image is linked via Google Cloud Storage, but the link has expired. Nevertheless, we can still extract crucial information, such as the storage bucket (`ideh-website-storage1`) and the object's name (`01koR8gU0hz7diuCyRnS1xH-3..v1569489628.png`).

```html
<html>

<head></head>

<body>
<h1 color="red">hacker memes</h1>
<img src="https://ffd6da3792c95a653c6b8c8b47c7e65f16c3c28adeccec6fbb7704d-apidata.googleusercontent.com/download/storage/v1/b/ideh-website-storage1/o/01koR8gU0hz7diuCyRnS1xH-3..v1569489628.png?jk=AanfhSA-BYWYRRsfnmsaHeMNY4aZjN_2fprd5ei1HGLxGPKvptAdyUqp-zvUm8S2Y8tb_14HKkXd5PYuFNIvSE0ZVK4pTyb7bSNJAgS91_qowkYvItN_9v7fUwYCWquv4c9SbeuUWsI6DC0frV_KmX6BcRjsfbbbc9PzHRlo-31Dm_ClR6iCsI2RBBc-MqRhyp9VaxL3YlAszIVDSUiu6eawC-bnJ7OhNA149QXpBcv0d1OiU7j1A6bB9qrHkjhK7r-fYT-akmkDvoOsFA3Nt9MlEcoF_YvHtKW-0M7K4vC3hB2M_nt-YzwnrGtlXdGY8D2jbYt4YuQvxArMyOe8T_5PZI65LJBcPyxZ0mWbI24tWLqyii31-wUf8V79ErWPICIaEgmD6TBUmyjtM7NOfZEJlj9bdO8Zfub3eBYozknn8-Z7M8esVHsYetvqjj5LtyTB-jBPSXydY8knMUw0ZFySxQ1yIWtWlcZ0jJGvfqIPdrDaZIMp1JKT4WsWMyoYk5rWHM18EXl6jY-B3PM4DjeCLauZ9E5lcFepK6vC8vtHiWLJAfe_BqyVrvJhmC2f98IJG8YUCs8L01mhOHvy-9jn9fXrJzSYD2JQOTRGRQHYA1hS0cchbfuywEhw-QGluMVoPDkkgzmnm5cn80HEoPV377sj0wATLudte_zPGUgyxm2dsBOqbxgtZaFiY7j-fBDqz-qEPVAiuupHnGuxRJn47KXmeEdfoqodyqoBtvU5YMnJkgiebSzgiZfpNHX0gGaugNr3ryRVszTRHvGYE8wzUHvgLvn-LBLNlLDgyicrddNNswu0syPTIyntK-PYZGFrNeeffoZDZCv8aCx2AIQOhCyATPmAois-pcawKj14tKBcaSjkmYzuUW7yEt0x4rYR4RUsufD6NBbQYZmALfDuXiHwA0KfrQRhQcVi4ugomRS7pa7eiw6rWFY-7Ttjaj2K5k95T_nrWZbXormMtDq-lhScKYqAbcvHyAqc3nfkGstMy2p-zmaNiaiSq5m95bi28cUje89FqT9L6zFXQ4-l4SID_alQmkXYeSetzk61gu8bt35z-c2j3gbrrEap0FXRhIlVNL-vaZ4yL0S5zHDrAxZphxeg74NqUaQE7H2f62atF5tH88QAu4SUaS0zKsqozFv6aZUA16GHgl73sQyr3YHUBMwbvGg&isca=1" />
</body>
</html>
```
After attempting various methods and consulting Google Cloud documentation, I opted for the Google Cloud CLI to list the bucket's content, revealing numerous images:

```bash
~/$ gcloud storage ls gs://ideh-website-storage1
gs://ideh-website-storage1/01koR8gU0hz7diuCyRnS1xH-3..v1569489628.png
gs://ideh-website-storage1/02C20007C81-cuPnCZ8L.png7C02C02C21402C20002B0.02C0.02C2140.02C2000.0_UY1000_.png
gs://ideh-website-storage1/10-funny-stereotypes-about-Hackers-blackMORE-Ops-10.jpg
gs://ideh-website-storage1/1622756237043.jpg
gs://ideh-website-storage1/1641381371943-930b3e67-be36-4168-b6c8-8b9a15a60e9d-image.png
gs://ideh-website-storage1/1651459398120.png
gs://ideh-website-storage1/20007C81-cuPnCZ8L.png7C02C02C21402C20002B0.02C0.02C2140.02C2000.0_AC_UY1000_.png
gs://ideh-website-storage1/200w_s.gif
gs://ideh-website-storage1/25e0063ff8bd10024c0209f68c114b6e122d960f9b051af4fec832f5771c00d5.jpg
gs://ideh-website-storage1/352597245a95c238a62f376c7727e351eb3d0c24_hq.jpg
gs://ideh-website-storage1/593ddd5144771704a6f3d55fcd0cc758e57ef11f.jpeg
gs://ideh-website-storage1/6156174380a46a9edc8584f6_fixing20bug201.jpg
gs://ideh-website-storage1/6f110b8b25cc515a2c301d2caff68415161c5bb8_00.jpg
gs://ideh-website-storage1/77dadce7196f873bcd508238855018b3530b4803454d7fe7c920924db01f7539.jpg
gs://ideh-website-storage1/866b557b30719e2f9b3586c5278264b392ac3b0b_hq.jpg
gs://ideh-website-storage1/893b8dfabd234416aaf8c6e8bfbc4cf51de876fe.jpeg
gs://ideh-website-storage1/ANLem4asbC2orrce3pxZH2p7kWP_VM7d2wq5213PkXVCs64-c-mo.jpg
gs://ideh-website-storage1/C_dnfIBXsAEjqio.jpg
gs://ideh-website-storage1/DSzVoGhW0AIPGa9.jpg
gs://ideh-website-storage1/DldDYclXcAAXv0t.jpg
gs://ideh-website-storage1/Esq5DLbXcAIyo-s.png
gs://ideh-website-storage1/FDEUZHDIU28368726.png
gs://ideh-website-storage1/Hacker-Meme-og.png
gs://ideh-website-storage1/Hackers-use-Memes-on-Twitter-to-secretly-send-commands-to-malware.jpg
gs://ideh-website-storage1/Meme_Hackers-1-scaled.jpg
gs://ideh-website-storage1/Security-meme-8-2.png
gs://ideh-website-storage1/WhiteHat-Hacker-Meme.jpeg
gs://ideh-website-storage1/b22.jpg
gs://ideh-website-storage1/edcf8114fb694908d567b96368b01a3ae4ee812a8d934a8f9043c53b6411b25f_1.jpg
gs://ideh-website-storage1/ef7f075d5c755fa74eb67d4389a527933e67d8325dd328583c3777313e750599.jpg
gs://ideh-website-storage1/f58bca34a94656bde962459fdd484336988813941a1f83d33ceea0ffe081bb1f_1.jpg
gs://ideh-website-storage1/f58e09ab24f0a9bee0c00290af629794e4755e56077735109d9c5d13c3641394.jpg
gs://ideh-website-storage1/fcpsmallwall_textureproduct750x1000.jpg
gs://ideh-website-storage1/flat750x075f-pad750x1000f8f8f8.u2.jpg
gs://ideh-website-storage1/hacker.png
gs://ideh-website-storage1/iStock-873960136-800x445.jpg
gs://ideh-website-storage1/il_570xN.4371112062_ra47.jpg
gs://ideh-website-storage1/image.jpeg
gs://ideh-website-storage1/image10.jpeg
gs://ideh-website-storage1/image11.jpeg
gs://ideh-website-storage1/image12.jpeg
gs://ideh-website-storage1/image13.jpeg
gs://ideh-website-storage1/image14.jpeg
gs://ideh-website-storage1/image15.jpeg
gs://ideh-website-storage1/image16.jpeg
gs://ideh-website-storage1/image17.jpeg
gs://ideh-website-storage1/image18.jpeg
gs://ideh-website-storage1/image19.jpeg
gs://ideh-website-storage1/image2.jpeg
gs://ideh-website-storage1/image20.jpeg
gs://ideh-website-storage1/image3.jpeg
gs://ideh-website-storage1/image4.jpeg
gs://ideh-website-storage1/image5.jpeg
gs://ideh-website-storage1/image6.jpeg
gs://ideh-website-storage1/image7.jpeg
gs://ideh-website-storage1/image8.jpeg
gs://ideh-website-storage1/image9.jpeg
gs://ideh-website-storage1/images.jpg
gs://ideh-website-storage1/images10.jpg
gs://ideh-website-storage1/images11.jpg
gs://ideh-website-storage1/images12.jpg
gs://ideh-website-storage1/images13.jpg
gs://ideh-website-storage1/images14.jpg
gs://ideh-website-storage1/images15.jpg
gs://ideh-website-storage1/images16.jpg
gs://ideh-website-storage1/images17.jpg
gs://ideh-website-storage1/images18.jpg
gs://ideh-website-storage1/images19.jpg
gs://ideh-website-storage1/images2.jpg
gs://ideh-website-storage1/images20.jpg
gs://ideh-website-storage1/images21.jpg
gs://ideh-website-storage1/images22.jpg
gs://ideh-website-storage1/images23.jpg
gs://ideh-website-storage1/images24.jpg
gs://ideh-website-storage1/images25.jpg
gs://ideh-website-storage1/images26.jpg
gs://ideh-website-storage1/images27.jpg
gs://ideh-website-storage1/images28.jpg
gs://ideh-website-storage1/images29.jpg
gs://ideh-website-storage1/images3.jpg
gs://ideh-website-storage1/images30.jpg
gs://ideh-website-storage1/images4.jpg
gs://ideh-website-storage1/images5.jpg
gs://ideh-website-storage1/images6.jpg
gs://ideh-website-storage1/images7.jpg
gs://ideh-website-storage1/images8.jpg
gs://ideh-website-storage1/images9.jpg
gs://ideh-website-storage1/meme-dev-humor-being-a-hacker-these-days-30.jpg
gs://ideh-website-storage1/meme-thumbnail4-hacker-meme.png
gs://ideh-website-storage1/modele-meme-drole-faux-hacker_23-2149617083.jpg
gs://ideh-website-storage1/programmerhumor-io-databases-memes-backend-memes-b67943d673dba31.jpg
gs://ideh-website-storage1/programmerhumor-io-programming-memes-1ca61539a51276b-758x1075.jpg
gs://ideh-website-storage1/programmerhumor-io-programming-memes-af82db691e237b3.png
gs://ideh-website-storage1/programmerhumor-io-programming-memes-c66a9fc7a932255.jpg
```

Afterward, I downloaded the entire bucket using the command:
```bash
~/$ gsutil -m cp -R gs://ideh-website-storage1 .
```
And then went through the images until I found this image that contained the flag:
![](https://github.com/ihuomtia/ctf-writeups/blob/main/IDEH-2024/imgs/FDEUZHDIU28368726.png?raw=true)

> [NOTE]
> In the initial hours of the challenge, there were permission issues, preventing public access to this storage bucket. After reporting the problem, the staff promptly corrected the error and notified all participants.
