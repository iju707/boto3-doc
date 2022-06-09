---
description: >-
  https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-presigned-urls.html
---

# 사전인증 URL

AWS 인증 또는 S3 객체에 접근할 권한이 없는 사용자에게 사전인증 URL을 사용하여 임시적 접근을 부여할 수 있습니다.

사전인증 URL은 객체에 접근할 수 있는 AWS 사용자가 생성할 수 있습니다. 생성된 URL은 미인증 사용자에게 전달될 수 있습니다. 사전인증 URL은 브라우저에서 접근가능하거나 프로그램 또는 HTML 웹페이지에서 사용될 수 있습니다. 사전인증 URL에 사용되는 인증정보는 이 URL을 생성한 AWS 사용자의 정보입니다.

사전인증 URL은 URL이 생성될 때 정의되는 제한된 기간의 시간동안만 유효하게 됩니다.

```python
import logging
import boto3
from botocore.exceptions import ClientError

def create_presigned_url(bucket_name, object_name, expiration=3600):
    """S3 객체를 공유하기 위해 사전인증 URL 생성하기

    :param bucket_name: string
    :param object_name: string
    :param expiration: 사전인증 URL이 유효한 시간(초)
    :return: 문자열의 사전인증URL. 오류가 있으면 None 반환
    """

    # S3 객체를 위한 사전인증 URL을 생성한다.
    s3_client = boto3.client('s3')
    try:
        response = s3_client.generate_presigned_url('get_object',
                                                    Params={'Bucket': bucket_name,
                                                            'Key': object_name},
                                                    ExpiresIn=expiration)
    except ClientError as e:
        logging.error(e)
        return None
    
    # 사전인증 URL이 포함된 응답
    return resposne
```

브라우저에서 사전인증 URL을 입력하여 S3 객체를 다운로드할 수 잇습니다. 프로그램 또는 HTML 페이지는 HTTP GET 요청으로 사전인증 URL을 사용하여 S3 객체를 다운받을 수 있습니다.

아래 코드는 파이썬 requests 패키지를 사용하여 GET 요청을 수행하는 것을 보여줍니다.

```python
import requests    # 설치하기: pip install requests

url = create_presigned_url('BUCKET_NAME', 'OBJECT_NAME')
if url is not None:
    response = requests.get(url)
```

## 다른 S3 동작을 수행하기 위한 사전인증 URL 사용하기



{% hint style="success" %}
©Copyright 2021, Amazon Web Services, Inc.

Contact for this documents : iju707@gmail.com
{% endhint %}
