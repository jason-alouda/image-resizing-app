# resizing app

This is a template for a resizing app. The COS component will create a COS (Cloud Object Storage) bucket. Images uploaded to that bucket will then be resized by the SCF (Serverless Cloud Function) and stored in the destination bucket as pre-determined by the user in the .env file.

&nbsp;

1. [Prepare](#Prepare)
2. [Download](#Download)
3. [Bootstrap](#Bootstrap)
4. [Deploy](#Deploy)
5. [Remove](#Remove)

&nbsp;

### Prepare

Before all below steps, you should install
[Serverless Framework](https://www.github.com/serverless/serverless) globally:

```bash
$ npm i serverless -g
```

### Download

Severless cli is very convenient, it can download templates in any github
project which should contain `serverless.yml` file.

```bash
$ serverless init -t resizing-app
```

### Bootstrap

Copy `.env.example` file to `.env` in project root:

Add the access keys of a
[Tencent CAM Role](https://console.cloud.tencent.com/cam/capi) with
`AdministratorAccess` in the `.env` file, like below:

```dotenv
# Your Tencent Cloud account API key credentials from https://console.cloud.tencent.com/cam/capi
# If you don't have a Tencent Cloud account already, sign up here https://intl.cloud.tencent.com/register
TENCENT_SECRET_ID=1234
TENCENT_SECRET_KEY=4321

# Please replace with your APPID. 请替换为您的 APPID
appid = 1234

# Please replace with the region where your COS bucket located. 请替换为您bucket 所在的地域
region = "XXX"

# Please replace with the name of the bucket where the compressed images are stored
resized_bucket = "XXX"
```

This template uses PIL (Python Imaging Library) as dependency. You must first install it using the CLI command:

```bash
$ pip install Pillow
```

### Deploy

```bash
$ sls deploy

serverless ⚡ framework

cos-bucket:
  region:        ap-guangzhou
  bucket:        cos-bucket-xxx
  cosOrigin:     cos-bucket-xxx.cos.ap-guangzhou.myqcloud.com
  url:           https://cos-bucket-xxx.cos.ap-guangzhou.myqcloud.com
  vendorMessage: null

resizing-scf:
  functionName:  resizing-scf
  description:   This demo need to config COS trigger. When object has been upload to COS bucket, the SCF will download it to /tmp and compress it. 本示例配置了 COS 触发器，当有文件上传到对应的 COS bucket时，会触发云函数执行，并把文件下载到本地临时目录，用做后续处理
  namespace:     default
  runtime:       Python2.7
  handler:       Thumbnail.main_handler
  memorySize:    128
  lastVersion:   $LATEST
  traffic:       1
  triggers:
    cos:
      - cos-bucket-xxx.cos.ap-guangzhou.myqcloud.com
  vendorMessage: null

29s › resizing › Success
```

### Remove

```bash
$ sls remove

serverless ⚡ framework

15s › resizing › Success
```
