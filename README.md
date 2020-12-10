# Service Links
- *IdentityService*: https://localhost:5001
- *NotificationService*: https://localhost:6001
- *ChatService*: https://localhost:7001
- *UsersService*: https://localhost:8001
- *AdvertisingService*: https://localhost:9001
- *SignalingService*: https://localhost:10001

### Please ensure ALL Services are started before testing them. They are best tested in this order
1. IdentityService (Request and Verify OTP)
2. UsersService (Update Info)
3. NotificationService (Update Notification Token)
4. The rest are fine.

# IdentityService
## Describe
#### Command
```bash
grpcurl -insecure localhost:5001 describe
```
## RequestOTP
Requests a one-time-password
#### Command
```bash
grpcurl -insecure -d '{"phoneNumber":"+6594509747"}' localhost:5001 services.Identity/RequestOTP
```
#### Expected response
```JSON
{
  "isSuccessful": true,
  "timeLeft": 60
}
```
## VerifyOTP
Verifies a one-time-password, if you are a first time user, your account will be automatically created.
#### Command
```bash
grpcurl -insecure -d '{"phoneNumber":"+6594509747", "otp":"7138865"}' localhost:5001 services.Identity/VerifyOTP
```
#### Expected response
```JSON
{
  "isSuccessful": true,
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOiJIM3B1czNKNmZyVnJmNG9LY29LQWlJWmh0QUwyIiwiY2xhaW1zIjp7InVpZCI6IkgzcHVzM0o2ZnJWcmY0b0tjb0tBaUlaaHRBTDIifSwiaXNzIjoiZmlyZWJhc2UtYWRtaW5zZGstd3A5cGtAbWNzdi1maXJlYmFzZS1zZXJ2aWNlLmlhbS5nc2VydmljZWFjY291bnQuY29tIiwic3ViIjoiZmlyZWJhc2UtYWRtaW5zZGstd3A5cGtAbWNzdi1maXJlYmFzZS1zZXJ2aWNlLmlhbS5nc2VydmljZWFjY291bnQuY29tIiwiYXVkIjoiaHR0cHM6Ly9pZGVudGl0eXRvb2xraXQuZ29vZ2xlYXBpcy5jb20vZ29vZ2xlLmlkZW50aXR5LmlkZW50aXR5dG9vbGtpdC52MS5JZGVudGl0eVRvb2xraXQiLCJleHAiOjE2MDYyNDM1MzAsImlhdCI6MTYwNjIzOTkzMH0.zNZ1tjbp30WxTe_oOtdOsOJI5FklTRTkFxN2YBCx5J3p9jxodmvLtOpQ8gUjrn9BZZW7ld8Cx9jzfgSWvMdm7yac22wDynKhJNF2uu0BOcedCZjcfzZ99VFamxkgOIE-ySXbo30Qj67h_TqntzVw4RCeoCrr8q5v3IZ0hn72jLk0YmRo6OQyob6herecyW8Y8LEwY9Z619fRLmPUE5TiCS15Twr-7pRFVxo4nZ-urIEtzqQ6b2FPOTL67A2LNrHG41dWhhEopw2nDd0jPnLI1Gb47Oz3QaJpTScxXEmDOCPQmh0F9wewDv74VJeBu2Udu1GGfjgaDnk7vfWD5Ly9Ag"
}
```
## UserSignUpListener
Event stream to inform listeners that a user has signed up
#### Command
```bash
grpcurl -insecure -d '{}' localhost:5001 services.Identity/UserSignUpListener
```
#### Expected response (stream)
```JSON
{
  "userid": "newUser.Uid",
  "phoneNumber": "+6594509747"
}
```
## VerifyToken
Verifies a **Firebase** token to demonstrate Authorization support
#### Command
```bash
grpcurl -insecure -H 'Authorization:Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImI5ODI2ZDA5Mzc3N2NlMDA1ZTQzYTMyN2ZmMjAyNjUyMTQ1ZTk2MDQiLCJ0eXAiOiJKV1QifQ.eyJ1aWQiOiJqRVdueHBxeERVVlkweEVKZHFmZVZ6eHFUN3AxIiwiaXNzIjoiaHR0cHM6Ly9zZWN1cmV0b2tlbi5nb29nbGUuY29tL21jc3YtZmlyZWJhc2Utc2VydmljZSIsImF1ZCI6Im1jc3YtZmlyZWJhc2Utc2VydmljZSIsImF1dGhfdGltZSI6MTYwNzM5ODkxMSwidXNlcl9pZCI6ImpFV254cHF4RFVWWTB4RUpkcWZlVnp4cVQ3cDEiLCJzdWIiOiJqRVdueHBxeERVVlkweEVKZHFmZVZ6eHFUN3AxIiwiaWF0IjoxNjA3Mzk4OTExLCJleHAiOjE2MDc0MDI1MTEsInBob25lX251bWJlciI6Iis2NTgxMzg3MzczIiwiZmlyZWJhc2UiOnsiaWRlbnRpdGllcyI6eyJwaG9uZSI6WyIrNjU4MTM4NzM3MyJdfSwic2lnbl9pbl9wcm92aWRlciI6ImN1c3RvbSJ9fQ.jtBWrc9aP_DS9j10NxQZmm7c2oPZQ7HrQRZfhWHatcqN6QiTuCH8qsp6ypkhhtXMDYZFEaEjP_WTa2_10LbPVLgtFS82GPhMdfSRgaTtEPvt8nlM1At0jwApSdRz-SmlU1dBsyabHZPVVL9CTuf-PzM5eBSUWO2keIFvcd7ohoSMM51ddgZUko17HZ2yN8E0q6dVB2pqeiza7AcLbXsdy1otrTfqNm7pwaG1xDbWBDgY4tt4LVAoNiKAfJQ8JhUMC_Q3WYAsDWrxZTO_vF-O31RN3DpW60q4Tiw6KFJYvCHWoc6XqaL8piVwrRPfRiC7gf7jdLPdqdUDNkx-qdbDdg' -d '{}' localhost:5001 services.Identity/VerifyToken
```
#### Expected response
```JSON
{
  "success": true
}
```

# NotificationService
Allows services to send notifications without firebase integration.
## Describe
#### Command
```bash
grpcurl -insecure localhost:6001 describe
```
#### Expected Response
```bash
services.Notification is a service:
service Notification {
  rpc SendNotificationByToken ( .services.TokenNotificationRequest ) returns ( .services.TokenNotificationResponse );
  rpc SendNotificationByTopic ( .services.TopicNotificationRequest ) returns ( .services.TopicNotificationResponse );
  rpc SendNotificationByUserId ( .services.UserIdNotificationRequest ) returns ( .services.UserIdNotificationResponse );
  rpc UpdateUserToken ( .services.UpdateUserTokenRequest ) returns ( .services.UpdateUserTokenResponse );
}
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}
```

## SendNotificationByToken
Sends notification by user token
#### Command
```bash
grpcurl -insecure -d '{"token":"", "title": "test", "message":"test"}' localhost:6001 services.Notification/SendNotificationByToken
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}
```
## SendNotificationByTopic
Sends notification by a topic, useful for announcements
#### Command
```bash
grpcurl -insecure -d '{"topic":"", "title": "test", "message":"test"}' localhost:6001 services.Notification/SendNotificationByTopic
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}
```
## SendNotificationByUserId
Sends notifications by the user id, looks it up from the database.
#### Command
```bash
grpcurl -insecure -d '{"userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2", "title": "test", "message":"test"}' localhost:6001 services.Notification/SendNotificationByUserId
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}
```
## UpdateUserToken
Updates the user token in the database and pairs it with a userid so we can use SendNotificationByUserId
#### Command
```bash
grpcurl -insecure -d '{"userid":"", "token": "token"}' localhost:6001 services.Notification/UpdateUserToken
```
#### Expected Response
```JSON
{
  "success": true
}
```

# ChatService
Main part of the application and microservices ecosystem, handles chats and group creation.
## Describe
#### Command
```bash
# you need to run the bundled TestChatService application
# open it and run it in visual studio.
# available inputs are !cancel and typing anything will send a message to yourself as a demo.
```
#### Expected Response
```bash
services.Chat is a service:
service Chat {
  rpc EventStream ( stream .services.Event ) returns ( stream .services.Event );
  rpc GetAllMessages ( .services.AllMessagesRequest ) returns ( .services.AllMessagesResponse );
  rpc GetUnreadMessages ( .services.NewMessagesRequest ) returns ( .services.NewMessagesResponse );
}
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}
```

## EventStream
Main event stream, when users subscribe to this, all of their new messages and events will be streamed here
#### Command
```bash
grpcurl -insecure -d @ localhost:7001 services.Chat/EventStream <<EOM
```
#### Stream Examples
```JSON
// typical message
{
    "message": {
        "id": "1",
        "receiverUserId": "WDP4zThKLvVrhLhM2coqVIrVmik2",
        "messageStatus": 0,
        "datePostedUnixTimestamp": 1000,
        "message": "Ligma"
        },
    "senderInfo":{
        "userid": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
        "isInit": false
    }
}
// test reply
{
    "message": {
        "id": "1",
        "receiverUserId": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
        "messageStatus": 0,
        "datePostedUnixTimNone"
        },
    "senderInfo":{
        "userid": "WDP4zThKLvVrhLhM2coqVIrVmik2",
        "isInit": false
    }
}
// on initialization (first call)
{
    "senderInfo":{
        "userid": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
        "isInit": true
    }
}

{
    "senderInfo":{
        "userid": "WDP4zThKLvVrhLhM2coqVIrVmik2",
        "isInit": true
    }
}
```
#### Expected Response
```JSON
{
   "message":{
      "receiverUserId":"RzNObuAzmRhnG9EWVEKFaP9bnSe2",
      "datePostedUnixTimestamp":"1606336614",
      "message":"ttttt"
   },
   "senderInfo":{
      "userid":"WDP4zThKLvVrhLhM2coqVIrVmik2"
   }
}
  
```
## GetAllMessages
Gets all the user's messages, useful for first time app launches on device.
#### Command
```bash
grpcurl -insecure -d '{"userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"}' localhost:7001 services.Chat/GetAllMessages
```
#### Expected Response
```JSON
{
  "message": [
    {
      "id": "a8cb2a37-8192-4a42-89cb-c483bdce000e",
      "receiverUserId": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
      "message": "what's up"
    },
    {
      "id": "3aee8c80-b861-495a-8ca7-6efafb834459",
      "receiverUserId": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
      "message": "I am really gay"
    }
  ]
}
```
## GetUnreadMessages
Gets all unread messages, used when app after app is launched after a period of inactivity
#### Command
```bash
grpcurl -insecure -d '{"userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"}' localhost:7001 services.Chat/GetUnreadMessages
```
#### Expected Response
```JSON
{
  "message": [
    {
      "id": "a8cb2a37-8192-4a42-89cb-c483bdce000e",
      "receiverUserId": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
      "message": "what's up"
    },
    {
      "id": "3aee8c80-b861-495a-8ca7-6efafb834459",
      "receiverUserId": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
      "message": "I am really gay"
    }
  ]
}
```
## GetGroupInfo
Gets the group chat information
#### Command
```bash
grpcurl -insecure -d '{"groupId":"3aee8c80-b861-495a-8ca7-6efafb834459"}' localhost:7001 services.Chat/GetGroupInfo
```
#### Expected Response
```JSON
{
  "title": "group",
  "groupId": "3aee8c80-b861-495a-8ca7-6efafb834459",
  "groupImage":{
    "mediaUrl": "www.google.com",
    "mimeType": "image/jpeg",
    "sizeBytes": 12345678
  }
}
```

# UsersService
Service that upserts and provides user information, nodejs because cloud firestore only supports node.
## Describe
#### Command
```bash
cd /path/to/project-root
grpcurl -import-path ./Protos -proto user.proto describe
```
#### Expected Response
```bash
services.User is a service:
// User service definition
service User {
  // get individual user info
  rpc GetUserInfo ( .services.GetUserInfoRequest ) returns ( .services.GetUserInfoResponse );
  // executed right after sign up/OTP, allows the user to setup more details like BIO and profile picture, also users can make use of this when updating profiles
  rpc UpdateProfile ( .services.UpdateProfileRequest ) returns ( .services.UpdateProfileResponse );
}
```
## GetUserInfo
Get the user information
#### Command
```bash
grpcurl -plaintext -import-path ./Protos -proto user.proto -d '{"userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"}' localhost:8001 services.User/GetUserInfo
```
#### Expected Response
```JSON
{
  "userid": "RzNObuAzmRhnG9EWVEKFaP9bnSe2",
  "phoneNumber": "+6594509747",
  "userName": "_LLJY",
  "bio": "just a person",
  "isExists": true
}
```
## UpdateProfile
Upserts the user profile information
#### Command
```bash
grpcurl -plaintext -import-path ./Protos -proto user.proto -d '{"userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2", "profilePictureUri": "","bio": "just a person", "username": "_LLJY", "phoneNumber": "+6594509747" }' localhost:8001 services.User/UpdateProfile
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}

```
# AdvertisingService
Service that provides advertisments for monetization
## Describe
#### Command
```bash
grpcurl -insecure localhost:9001 describe
```
#### Expected Response
```bash
services.Advertising is a service:
service Advertising {
  rpc DeleteAdvertisement ( .services.DeleteAdvertRequest ) returns ( .services.GenericResponse );
  rpc GetAdvertisements ( .services.AdvertsList ) returns ( .services.AdvertsList );
  rpc UpsertAdvertisement ( .services.Advert ) returns ( .services.GenericResponse );
}
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}
```
## DeleteAdvertisement
Self explanatory
#### Command
```bash
grpcurl -insecure -d '{"advertId":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"}' localhost:9001 services.Advertising/DeleteAdvertisement
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}
```
## GetAdvertisements
Self explanatory
#### Command
```bash
grpcurl -insecure -d '{}' localhost:9001 services.Advertising/GetAdvertisements
```
#### Expected Response
```JSON
{
  "advert": [
    {
      "title": "testAd",
      "message": "hey there"
    }
  ]
}
```
## UpsertAdvertisement
Upserts (updates or inserts) an advertisement
#### Command
```bash
grpcurl -insecure -d '{"title":"testAd", "message": "hey there", "imageUrl": null}' localhost:9001 services.Advertising/UpsertAdvertisement
```
#### Expected Response
```JSON
{
  "isSuccessful": true
}
```

# SignalingService
Acts as a middleman between two clients to initiate a WebRTC Call.
## Describe
#### Command
```bash
grpcurl -insecure localhost:10001 describe
```
#### Expected Response
```bash
services.Signaling is a service:
service Signaling {
  rpc CallStream ( stream .services.Signal ) returns ( stream .services.Signal );
}
grpc.reflection.v1alpha.ServerReflection is a service:
service ServerReflection {
  rpc ServerReflectionInfo ( stream .grpc.reflection.v1alpha.ServerReflectionRequest ) returns ( stream .grpc.reflection.v1alpha.ServerReflectionResponse );
}
```
## CallStream
Bi-directional streaming from client to client to exchange WebRTC Information.
#### Command
```bash
# you need to run the bundled TestSignalingService application
# open it and run it in visual studio.
# available inputs are !SignalOffer and !ICERequest
```
#### Stream Response Examples
```JSON
// Signal Offer
{
   "signalOffer":{
      "type":"signal-offer",
      "name":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"
   },
   "recieverId":"RzNObuAzmRhnG9EWVEKFaP9bnSe2",
   "senderInfo":{
      "userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"
   }
}
// ICERequest
{
   "ICECandidateRequest":{
      "type":"ice-request",
      "target":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"
   },
   "recieverId":"RzNObuAzmRhnG9EWVEKFaP9bnSe2",
   "senderInfo":{
      "userid":"RzNObuAzmRhnG9EWVEKFaP9bnSe2"
   }
}
```