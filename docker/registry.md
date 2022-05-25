```
### Catagories ###
curl -u <USER> -X GET https://<END POINT>:<PORT>/v2/_catalog

### Tag List ###
curl -u <USER> -X GET https://<END POINT>:<PORT>/v2/<IMAGE>/tags/list

### Delete Image ###
curl -u <USER> -X DELETE https://<END POINT>:<PORT>/v2/<IMAGE>/manifests/<DIGEST>
```
