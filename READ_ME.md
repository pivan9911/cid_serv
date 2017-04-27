

#CidService


##CidService  user  REST-interface below.
Note, database needs be initialized as in CidService/sql/fire_CID_MAPPING.sql

### Basic reading of singular values(HTTP_GET)

####Get anonymized value for cidacc and categoryid.

   **HTTP-GET  /cidservice/cid/{cidacc}/category/{categoryid}/internalMask**<br/>
  (Content-type: Application/json ,  [ Error-code: NOT-FOUND] )<br/>
Returns value of internalMask.
<p/>

####Get cid value for im_id and categoryid.
   **HTTP-GET  /cidservice/internalMask/{im_id}/category/{categoryid}/cid**<br/>
   (Content-type.  Application/json  ,   [Error-code: NOT-FOUND ] )<br/>
Returns value of cid.
<p/>


#### Creation of entry,  with HTTP_POST.
   **HTTP-POST  /cidservice** <br/>
   request-body {"cid"; cid_value,  "category": category_value}<br/>
   (Content-type.  Application/json  [Error-code: PRECONDITION_FAILED ] )<br/>
<p/>

#### Deletion of entry,  with HTTP_DELETE.
   **HTTP-DELETE  /cidservice/cid/{cid}/category/{categoryid}**<br/>
   (Content-type.  Application/json,  [Error-code: NOT-FOUND ]   )<br/>
<p/>


##### Examples

Get Anonymized value for cid(GIM2)=87654321 category=Bank_Account.
* curl --user firedev:firedev 'http://localhost:9097/cidservice/cid/87654323/category/GIM2_ID/internalMask'  -H 'Content-Type: text/plain'
* curl --user firedev:firedev 'http://localhost:9097/cidservice?cid=87654321&category=Bank_Account&active=true&property=internalMask'  -H 'Content-Type: text/plain'
<br/>
<br/>

When https is configured, use certificates in src/test/resources/springsecurity_certs.
* curl -k --cert ./rod.p12       'https://localhost:9098/cidservice/cid/87654323/category/GIM2_ID/internalMask'
* wget --auth-no-challenge --no-check-cert --certificate=./rod.p12 'https://localhost:9098/cidservice/cid/87654323/category/GIM2_ID/internalMask'

<br/>
<br/>

Lookup CID-value for anonymized internalMask=00000001  category=Bank_Account
* curl --user firedev:firedev  'http://localhost:9097/cidservice/internalMask/00000001/category/Bank_Account/cid'   -H 'Content-Type: text/plain'
* curl --user firedev:firedev  'http://localhost:9097/cidservice?internalMask=00000001&category=Bank_Account&active=true&property=cid'  -H 'Content-Type: text/plain'
<br/>
<br/>

Create new CidMapping for CID-=99990001 Category=BankAccount
* curl  -X POST  --user firedev:firedev  -d '{"cid": "99990001", "category":"Bank_Account"}'   'http://localhost:9097/cidservice'   -H 'Content-Type: Application/json'
<br/>
<br/>

SoftDelete CidMapping for CID-=99990001 Category=BankAccount
* curl -i -X DELETE --user firedev:firedev    'http://localhost:9097/cidservice/cid/99990001/category/Bank_Account'
<br/>
<br/>



Get Anonymized value, from DEV-server,  for cid(GIM2)=87654321 category=Bank_Account.
* http://a301-6532-4031.ldn.swissbank.com:8095/cidservice/cid/87654321/category/Bank_Account/internalMask
* http://a301-6532-4031.ldn.swissbank.com:8095/cidservice/internalMask/00000001/category/Bank_Account_ID/cid
<br/>

##CidService  user  REST-interface, priority 2:

####Lookup a complete CidMapping-record.

   **HTTP-GET  /cidservice/cid/{cid_acc}/category/{categoryid}**<br/>
   (Content-type. JSON ,  [ Error-code: NOT-FOUND] )<br/><br/>
   { "id": "2022",  "cid": cid_acc,  "category": category_1, "IPID": ipid1,  "internalMask": internalMask_1,  "valid" : true,}
<p/>


#### Editing of CidMapping-record with specified id, property "valid".
   **HTTP-PUT /cidservice** <br/>
   request-body {  "id": "2022", "valid" : true }<br/>
   (Content-type.  Application/json  [Error-code: PRECONDITION_FAILED ] )<br/>
<p/>

####Lookup all CidMapping-records for given CID account.
   **HTTP-GET /cidservice/cid/{cid_acc}**<br/>
  (Content-type. JSON ,  [ Error-code: NOT-FOUND] )<br/>
  Returns CidMappings for cid_acc, e.g.<br/>
  [ http://localhost:9097/cidservice/cid/87654321/category/Bank_Account, http://localhost:9097/cidservice/cid/87654321/category/GIM2_ID ]

<p/>


##### Examples

See all properties of cid=87654321 Category=BankAccount <br/>
* curl  --user firedev:firedev 'http://localhost:9097/cidservice/cid/87654321/category/Bank_Account'  -H 'Content-Type: Application/json;pretty=true'
<br/>
<br/>

Set valid=true, for CidMapping id=1.<br/>
* curl -X PUT -d '{"id": "1", "valid": true }' --user firedev:firedev 'http://localhost:9097/cidservice'  -H 'Content-Type: Application/json;pretty=true'
<br/>
<br/>

Get all CidMappings for cid=87654321.<br/>
* curl  --user firedev:firedev 'http://localhost:9097/cidservice/cid/87654321'  -H 'Content-Type: Application/json;pretty=true'
<br/>
<br/>