# QR code generation problem in nhost

## Run project locally

```
nhost dev
```

Click URL:
http://localhost:1337/v1/functions/generate-qr?locationId=1&address=some-address&description=some-description

It will generate and display the QR code as a PNG.

It generates a QR with the contents of the passed parameters

## Run in production

Once deployed to nhost, you can access it like this:

https://veywluagclwbyvxhgycw.functions.eu-central-1.nhost.run/v1/generate-qr?locationId=1&address=some-address&description=some-description

It will generate _something_ and force to download. It looks like PNG, but we receive it as `application/octet` and the PNG content is not valid.

## Further investigation

It seems to generate garbage bytes into the stream. Normally a png file starts with the bytes

```
89 50 4E 47 0D 0A 1A 0A
```

which is 89 + 'PNG'.

In the deployed version it is

```
EF BF BD 50 4E 47 ...
```

EF BF BD + PNG

It is most probably related to the fact that nhost is deployed to AWS and Lambda has a wierd way to allow returning binary content:

https://adil.medium.com/how-to-send-an-image-as-a-response-via-aws-lambda-and-api-gateway-3820f3d4b6c8
