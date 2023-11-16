### UUID 
- UUID라는 고유한 값(네트워크 상에서 고유성이 보장되는 id를 만들기위한 표준 규약, Universally Unique IDentifier) 이 있는데 이것은 32자리의 16진수로 표현된다.  
- 암호화하기 위해 16진수로 바꾸는 가장 잘 알려진 툴이 TO_BASE64고 이이
- TO_BASE64(xx_id) :

SQL에서 `TO_BASE64` 함수는 데이터를 Base64로 인코딩하는 데 사용됩니다. Base64는 이진 데이터를 텍스트 형식으로 표현하는 데 사용되는 인코딩 방식 중 하나입니다. 이진 데이터는 0과 1로 이루어진 비트로 표현되는데, 이를 텍스트로 표현하면 가독성이 떨어지고 특수 문자 등이 문제가 될 수 있습니다. 그래서 Base64 인코딩은 이진 데이터를 ASCII 문자 집합으로 변환하여 텍스트로 표현하는 방법 중 하나입니다.

일반적으로 다음과 같은 상황에서 `TO_BASE64` 함수를 사용할 수 있습니다:

1. **이진 데이터 저장:** 이진 데이터(예: 이미지, 오디오, 비디오)를 데이터베이스에 저장할 때, 이진 데이터를 Base64로 인코딩하여 텍스트 형식으로 저장할 수 있습니다.

    ```sql
    INSERT INTO MyTable (ImageColumn) VALUES (TO_BASE64(CONVERT(varbinary(max), 'binary_data')));
    ```

2. **데이터 전송:** 이진 데이터를 웹 애플리케이션 등으로 전송할 때, Base64로 인코딩하여 텍스트 형식으로 변환하고 다시 디코딩하여 사용할 수 있습니다.

    ```sql
    SELECT TO_BASE64(ImageColumn) AS Base64Image FROM MyTable;
    ```

3. **문자열 처리:** 이진 데이터가 아니더라도, 어떤 이유로든 문자열을 Base64로 인코딩하여 처리해야 할 때 사용할 수 있습니다.

    ```sql
    SELECT TO_BASE64('Hello, World!') AS EncodedString;
    ```

기본적으로 `TO_BASE64`의 결과는 문자열입니다. 디코딩을 위해서는 `FROM_BASE64` 함수를 사용할 수 있습니다.
