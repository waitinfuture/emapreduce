# Sign signatures {#concept_uns_h5t_hfb .concept}

ECS performs identity authentication on each access request. Therefore, whether submitted through HTTP or HTTPS, a request must contain signature information.

ECS uses symmetric encryption based on an Access Key ID and Access Key Secret to verify the identity of a request sender. The Access Key ID and Access Key Secret are issued to users by Alibaba Cloud \(users can request and manage keys on the website of Alibaba Cloud\). The Access Key ID indicates the identity of the user. The Access Key Secret is the secret key used to encrypt the signature string and to verify the signature string on the server. It must be kept strictly confidential and only be known to Alibaba Cloud and authenticated users.

## Endpoint {#section_xtb_hwt_hfb .section}

Follow the instructions below to sign the signatures for your requests.

1.  Use request parameters to construct a Canonicalized Query String.
    1.  Sort parameters

        Sort all request parameters \(including common request parameters and user-defined parameters for the given request interfaces described in this document, excluding the Signature parameter mentioned in "Common request parameters"\) alphabetically by the parameter name.

        **Note:** If you use the GET method to submit requests, these parameters are included in the request URI that is the part after the question mark \(?\) following the ampersand \(&\) in the URI .

    2.  Encode parameters

        Encode the name and value of each request parameter. The names and values must undergo URL encoding using the UTF-8 character set. The URL encoding rules are as follows:

        -   Uppercase letters \(A–Z\), lowercase letters \(a–z\), numbers \(0–9\), hyphens \(-\), underscores \(\_\), periods \(.\), and tildes \(~\) are not encoded.

        -   Other characters are encoded in "%XY" format, with XY representing the character ASCII code in hexadecimal notation. The double quotation mark \("\) is coded as %22.

        -   Extended UTF-8 characters are encoded in “%XY%ZA… “

        -   Note that an English space is encoded as %20 instead of a plus sign \(+\).

        **Note:** This encoding method is similar to but different from the application/x-www-form-urlencoded MIME encoding algorithm \(such as the implementation of the java.net.URLEncoder class provided by the Java standard library\). You can use this encoding method directly by replacing the plus sign \(+\) in the encoded string with "%20", the asterisk \(\*\) with "%2A", and change "%7E" back to the tilde \(~\) to conform to the encoding rules described above. This algorithm can be implemented using the following percentEncode\(\) method:

        ```
        private static final String ENCODING = "UTF-8";
        private static String percentEncode(String value) throws UnsupportedEncodingException {
          return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null;
        }
        ```

    3.  Connect the encoded parameter names and values with equal signs \(=\).
    4.  Then, sort the parameter name and value pairs connected by equal signs in alphabetical order, and connect them with an ampersand \(&\) to produce the canonicalized query string.
2.  Follow the rules below to use the canonicalized query string to construct the string for signature calculation:

    ```
    StringToSign=
      HTTPMethod + “&” +
      percentEncode(“/”) + ”&” +
       percentEncode(CanonicalizedQueryString)
    ```

    In this formulation, HTTPMethod is the HTTP method \(such as GET\) used for request submission. percentEncode\(“/“\) is the encoded value \(%2F\) for the character “/“ according to the URL encoding rules described in 1.ii. percentEncode \(CanonicalizedQueryString\) is the encoded string for the canonicalized query string constructed in Step 1 according to the URL encoding rules described in 1.ii.

3.  Use the string for signature calculation to calculate the HMAC value of the signature based on RFC2104.

    **Note:** The key used for signature calculation is your secret access key adding the ampersand “&” \(ASCII:38\) and it is based on the SHA1 hashing algorithm.

4.  Encode the HMAC value into a string based on Base64 encoding rules to obtain the signature value.
5.  Add the obtained signature value to request parameters as the Signature parameter to complete the request signing process.

    **Note:** The obtained signature value requires URL encoding based on the RFC3986 rule like other parameters before it is submitted to the GPDB server as the final request parameter value.

    Take "DescribeDBInstances" as an example, the request URL before a signature is :

    ```
    http://ecs.aliyuncs.com/?TimeStamp=2016-02-23T12:46:24Z&Format=XML&AccessKeyId=testid&Action=DescribeRegions&SignatureMethod=HMAC-SHA1&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&SignatureVersion=1.0
    ```

    The corresponding StringToSign is:

    ```
    GET&%2F&AccessKeyId%3Dtestid&Action%3DDescribeRegions&Format%3DXML&SignatureMethod%3DHMAC-SHA1&SignatureNonce%3D3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&SignatureVersion%3D1.0&TimeStamp%3D2016-02-23T12%253A46%253A24Z&Version%3D2014-05-26
    ```

    Assume that the value of the Access Key Secret parameter is testsecret. Then the key used for HMAC calculation is testsecret&, and the calculated signature value is:

    ```
    CT9X0VtwR86fNWSnsc6v8YGOjuE=
    ```

    The signed request URL is \(with the Signature parameter added\):

    ```
    http://ecs.aliyuncs.com/?SignatureVersion=1.0&Action=DescribeRegions&Format=XML&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf&Version=2014-05-26&AccessKeyId=testid&Signature=CT9X0VtwR86fNWSnsc6v8YGOjuE%3D&SignatureMethod=HMAC-SHA1&TimeStamp=2016-02-23T12%3A46%3A24Z
    ```


