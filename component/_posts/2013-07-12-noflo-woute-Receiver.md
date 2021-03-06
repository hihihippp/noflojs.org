---
  title: "Receiver"
  library: "noflo-woute"
  layout: "component"

---

```coffeescript
EXPORT=REQUEST.IN:IN
EXPORT=SESSIONID.OUT:SID
EXPORT=EXTRACTRESPONSE.OUT:RESPONSE
EXPORT=REQUESTOBJECT.OUT:REQUEST
EXPORT=URL.OUT:URL
EXPORT=EXTRACTBODY.OUT:BODY
EXPORT=EXTRACTHEADERS.OUT:HEADERS
EXPORT=EXTRACTQUERY.OUT:QUERY

Request(Split) OUT -> IN SessionId(swiss/RandomUuid)
'../config/session_id_format.txt' -> IN SessionIdFormatPath(woute/ResolvePath) OUT -> IN ReadSessionIdFormat(ReadFile) OUT -> REGEXP RemoveSessionId(groups/RemoveGroups)
Request() OUT -> IN RemoveSessionId() OUT -> IN FirstInRequest(packets/First) OUT -> IN RequestResponseObject(Split)

'req' -> KEY ExtractRequest(objects/ExtractProperty)
RequestResponseObject() OUT -> IN ExtractRequest() OUT -> IN RequestObject(Split)
'res' -> KEY ExtractResponse(objects/ExtractProperty)
RequestResponseObject() OUT -> IN ExtractResponse()

'url' -> KEY ExtractUrl(objects/ExtractProperty)
'/' -> DELIMITER SplitUrl(SplitStr)
RequestObject() OUT -> IN ExtractUrl() OUT -> IN SplitUrl() OUT -> IN DecodeUri(woute/DecodeUri) OUT -> IN Url(packets/Compact)

'body' -> KEY ExtractBody(objects/ExtractProperty)
RequestObject() OUT -> IN ExtractBody()

'headers' -> KEY ExtractHeaders(objects/ExtractProperty)
RequestObject() OUT -> IN ExtractHeaders()

'query' -> KEY ExtractQuery(objects/ExtractProperty)
RequestObject() OUT -> IN ExtractQuery()

```