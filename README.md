# Super Smash Bros. Melee - "All Trophies" Speedrun Tracker (backend)


* * *


Backend API Documentation


## Tables


### Session Table

id  | lastUpdated                | deletedAt | t001 | t002 | ...t292 | b001  | b002 | ...b253
:-- | :--                        | :--       | :--  | :--  | :--     | :--   | :--  | :--
50  | '2022-12-28T01:38:01.289Z' | null      | true | true | false   | false | true | true


### Secret Table

id  | sessionId | secret
:-- | :--       | :--
24  | 50        | '0753'


### Socket Table

id  | sessionId | index
:-- | :--       | :--
34  | 50        | 2


## Endpoints


### Specific Endpoints:

* `GET /sessions`
  * Publically available
  * Returns array of sessions, excluding any that have a non-null value for the `deletedAt` field
* `GET /sessions?deletedAt=true`
  * Publically available
  * Returns array of all sessions
* `GET /sessions/:id`
  * Publically available
  * return session object
* `POST /sessions`
  * Will create a new session record, along with new related secret and socket records
  * Returns an object with the Session ID, Date, and a Secret
    * { id: 50, secret: '0752', lastUpdated: '2022-12-28T01:38:01.289Z' }
    * Frontend will store this returned object in local storage
* `PATCH /sessions/:id`
  * Must include the Session ID, the Secret, and a TXXX or BXXX value with boolean
    * `{ id: 50, secret: '0753', b004: true }`
  * All requests will validate the secret matches the session first and reject otherwise (401?)
    * Should there be a "guess" limit, or timeout?
  * All successful patches will also update the `lastUpdated` field with the current time stamp
  * All successful patches will also increment the related socket `index`
* `DELETE /sessions/:id`
  * Must include the Session ID and the Secret
    * `{ id: 50, secret: '0753' }`
  * All requests will validate the secret matches the session first and reject otherwise (401?)
    * Should there be a "guess" limit, or timeout?
  * Does not actually delete the record, instead sets a "deletedAt" date field to have a date value
* `GET /sockets/:id`
  * Should be a websocket
  * Anytime the socket's index changes, anyone listening to it should be notified so they can `GET /sessions/:id`


### Forbidden Endpoints:

The following endpoints marked with an ❌ should probaly 403 or 404 (basicaly anything not explicitly defined above shouldn't work).

Route           | GET  | PUT  | POST | PATCH | DELETE
:--             | :--: | :--: | :--: | :--:  | :--:
`/sessions`     | ✔    | ❌   | ✔   | ❌    | ❌
`/sessions/:id` | ✔    | ❌   | ❌   | ✔    | ✔
`/secrets`      | ❌    | ❌   | ❌   | ❌    | ❌
`/secrets/:id`  | ❌    | ❌   | ❌   | ❌    | ❌
`/sockets`      | ❌    | ❌   | ❌   | ❌    | ❌
`/sockets/:id`  | ✔    | ❌   | ❌   | ❌    | ❌


## Fields


### Session Table Fields

* `id`
  * Used in url of website, can be typed in to join someone's existing session
* `lastUpdated`
  * A JS ISO date string that is set by the server during initial POST and after any successful PATCH
  * '2022-12-28T01:38:01.289Z'
  * (new Date()).toISOString()
* `deletedAt`
  * A JS ISO date string that is set by the server during a DELETE
  * '2022-12-28T01:38:01.289Z'
  * (new Date()).toISOString()
* `t001`-`t292`
  * 292 boolean fields representing which of the 292 trophies have been acquired
* `b001`-`b253`
  * 253 boolean fields representing which of the 253 bonuses have been acquired


### Secret Table Fields

* `id`
  * Only used by DB joins
* `sessionId`
  * The related ID in the Session Table
* `secret`
  * A string of 4 numbers, '1234', '0000', '4862'
  * Initially the only the session creator has this secret, but they can share it with those they want to collaborate with


### Socket Table Fields

* `id`
  * Only used for DB joins
* `sessionId`
  * The related ID in the Session Table
* `index`
  * A number starting at 0 and incrementing as the related session record is patched


## Parameters

These would apply to all list routes, which in this case is just `/sessions`.

* `limit=100` - On listing routes, this limits the response to `n` items
* `offset=100` - The amount of items to skip to, used for pagination
* `sort=id` - Sort the results by any field that endpoint returns
* `deletedAt=true` - Will return all records, including records that have a deleted time stamp (not returned by default)
* Maybe filtering? With the fields we have I think sorting and pagination is probably good enough
  * `fieldName="exact match"`
  * `fieldName=~"containing match"
  * `lastUpdate=">2022-12-31T06:00:00.000Z" (filter to records updated after 12/31)


## Example Session Object

```json
{
  "id": 83,
  "lastUpdated": "2022-12-28T17:08:37.856Z",
  "deletedAt": null,
  "t001": false,
  "t002": false,
  "t003": false,
  "t004": false,
  "t005": false,
  "t006": false,
  "t007": false,
  "t008": true,
  "t009": false,
  "t010": false,
  "t011": false,
  "t012": false,
  "t013": false,
  "t014": false,
  "t015": true,
  "t016": true,
  "t017": false,
  "t018": false,
  "t019": false,
  "t020": true,
  "t021": false,
  "t022": true,
  "t023": true,
  "t024": false,
  "t025": false,
  "t026": false,
  "t027": false,
  "t028": false,
  "t029": false,
  "t030": false,
  "t031": false,
  "t032": false,
  "t033": false,
  "t034": false,
  "t035": false,
  "t036": false,
  "t037": false,
  "t038": false,
  "t039": true,
  "t040": true,
  "t041": true,
  "t042": true,
  "t043": false,
  "t044": false,
  "t045": false,
  "t046": false,
  "t047": true,
  "t048": false,
  "t049": false,
  "t050": false,
  "t051": false,
  "t052": false,
  "t053": false,
  "t054": false,
  "t055": true,
  "t056": false,
  "t057": false,
  "t058": false,
  "t059": false,
  "t060": false,
  "t061": false,
  "t062": false,
  "t063": false,
  "t064": false,
  "t065": true,
  "t066": false,
  "t067": true,
  "t068": false,
  "t069": false,
  "t070": false,
  "t071": false,
  "t072": true,
  "t073": true,
  "t074": true,
  "t075": true,
  "t076": false,
  "t077": true,
  "t078": false,
  "t079": false,
  "t080": false,
  "t081": false,
  "t082": false,
  "t083": false,
  "t084": true,
  "t085": false,
  "t086": false,
  "t087": false,
  "t088": true,
  "t089": false,
  "t090": false,
  "t091": true,
  "t092": false,
  "t093": false,
  "t094": false,
  "t095": false,
  "t096": false,
  "t097": false,
  "t098": false,
  "t099": false,
  "t100": false,
  "t101": true,
  "t102": false,
  "t103": false,
  "t104": false,
  "t105": true,
  "t106": true,
  "t107": false,
  "t108": false,
  "t109": false,
  "t110": false,
  "t111": false,
  "t112": true,
  "t113": true,
  "t114": false,
  "t115": false,
  "t116": false,
  "t117": false,
  "t118": false,
  "t119": false,
  "t120": true,
  "t121": false,
  "t122": true,
  "t123": true,
  "t124": false,
  "t125": true,
  "t126": false,
  "t127": false,
  "t128": false,
  "t129": false,
  "t130": false,
  "t131": false,
  "t132": true,
  "t133": false,
  "t134": true,
  "t135": false,
  "t136": false,
  "t137": false,
  "t138": true,
  "t139": false,
  "t140": true,
  "t141": false,
  "t142": true,
  "t143": true,
  "t144": true,
  "t145": false,
  "t146": true,
  "t147": false,
  "t148": false,
  "t149": false,
  "t150": false,
  "t151": false,
  "t152": false,
  "t153": false,
  "t154": false,
  "t155": false,
  "t156": true,
  "t157": false,
  "t158": false,
  "t159": false,
  "t160": true,
  "t161": false,
  "t162": false,
  "t163": true,
  "t164": true,
  "t165": true,
  "t166": false,
  "t167": false,
  "t168": false,
  "t169": true,
  "t170": false,
  "t171": false,
  "t172": false,
  "t173": false,
  "t174": true,
  "t175": false,
  "t176": false,
  "t177": false,
  "t178": false,
  "t179": true,
  "t180": false,
  "t181": false,
  "t182": false,
  "t183": false,
  "t184": true,
  "t185": false,
  "t186": false,
  "t187": false,
  "t188": true,
  "t189": false,
  "t190": false,
  "t191": false,
  "t192": false,
  "t193": true,
  "t194": false,
  "t195": true,
  "t196": false,
  "t197": true,
  "t198": false,
  "t199": false,
  "t200": false,
  "t201": false,
  "t202": false,
  "t203": false,
  "t204": false,
  "t205": false,
  "t206": false,
  "t207": false,
  "t208": false,
  "t209": true,
  "t210": false,
  "t211": false,
  "t212": false,
  "t213": false,
  "t214": true,
  "t215": false,
  "t216": false,
  "t217": false,
  "t218": true,
  "t219": false,
  "t220": false,
  "t221": false,
  "t222": false,
  "t223": false,
  "t224": false,
  "t225": true,
  "t226": true,
  "t227": false,
  "t228": false,
  "t229": false,
  "t230": false,
  "t231": false,
  "t232": false,
  "t233": false,
  "t234": false,
  "t235": false,
  "t236": false,
  "t237": false,
  "t238": false,
  "t239": false,
  "t240": false,
  "t241": false,
  "t242": false,
  "t243": true,
  "t244": false,
  "t245": false,
  "t246": false,
  "t247": false,
  "t248": true,
  "t249": true,
  "t250": false,
  "t251": false,
  "t252": false,
  "t253": false,
  "t254": false,
  "t255": true,
  "t256": false,
  "t257": false,
  "t258": false,
  "t259": false,
  "t260": true,
  "t261": false,
  "t262": true,
  "t263": true,
  "t264": false,
  "t265": false,
  "t266": true,
  "t267": true,
  "t268": false,
  "t269": false,
  "t270": true,
  "t271": false,
  "t272": false,
  "t273": false,
  "t274": false,
  "t275": false,
  "t276": false,
  "t277": false,
  "t278": false,
  "t279": true,
  "t280": false,
  "t281": false,
  "t282": false,
  "t283": false,
  "t284": false,
  "t285": false,
  "t286": false,
  "t287": false,
  "t288": true,
  "t289": false,
  "t290": false,
  "t291": false,
  "t292": true,
  "b001": false,
  "b002": false,
  "b003": false,
  "b004": false,
  "b005": true,
  "b006": false,
  "b007": false,
  "b008": true,
  "b009": false,
  "b010": false,
  "b011": true,
  "b012": false,
  "b013": false,
  "b014": false,
  "b015": false,
  "b016": true,
  "b017": true,
  "b018": false,
  "b019": true,
  "b020": false,
  "b021": true,
  "b022": false,
  "b023": false,
  "b024": false,
  "b025": false,
  "b026": false,
  "b027": false,
  "b028": false,
  "b029": false,
  "b030": false,
  "b031": false,
  "b032": false,
  "b033": false,
  "b034": false,
  "b035": false,
  "b036": false,
  "b037": false,
  "b038": true,
  "b039": false,
  "b040": false,
  "b041": false,
  "b042": false,
  "b043": false,
  "b044": false,
  "b045": false,
  "b046": false,
  "b047": false,
  "b048": false,
  "b049": false,
  "b050": false,
  "b051": true,
  "b052": true,
  "b053": false,
  "b054": false,
  "b055": false,
  "b056": false,
  "b057": false,
  "b058": false,
  "b059": false,
  "b060": false,
  "b061": true,
  "b062": true,
  "b063": true,
  "b064": false,
  "b065": false,
  "b066": false,
  "b067": false,
  "b068": false,
  "b069": true,
  "b070": false,
  "b071": false,
  "b072": false,
  "b073": true,
  "b074": false,
  "b075": true,
  "b076": false,
  "b077": false,
  "b078": false,
  "b079": false,
  "b080": false,
  "b081": false,
  "b082": false,
  "b083": true,
  "b084": false,
  "b085": false,
  "b086": true,
  "b087": false,
  "b088": false,
  "b089": false,
  "b090": false,
  "b091": false,
  "b092": false,
  "b093": false,
  "b094": false,
  "b095": false,
  "b096": false,
  "b097": false,
  "b098": false,
  "b099": false,
  "b100": false,
  "b101": false,
  "b102": false,
  "b103": false,
  "b104": false,
  "b105": true,
  "b106": false,
  "b107": false,
  "b108": false,
  "b109": true,
  "b110": false,
  "b111": false,
  "b112": true,
  "b113": false,
  "b114": true,
  "b115": true,
  "b116": false,
  "b117": true,
  "b118": false,
  "b119": false,
  "b120": false,
  "b121": false,
  "b122": true,
  "b123": false,
  "b124": false,
  "b125": true,
  "b126": false,
  "b127": false,
  "b128": false,
  "b129": false,
  "b130": false,
  "b131": false,
  "b132": false,
  "b133": false,
  "b134": true,
  "b135": true,
  "b136": false,
  "b137": false,
  "b138": false,
  "b139": true,
  "b140": false,
  "b141": false,
  "b142": true,
  "b143": false,
  "b144": false,
  "b145": false,
  "b146": false,
  "b147": false,
  "b148": false,
  "b149": false,
  "b150": true,
  "b151": false,
  "b152": false,
  "b153": true,
  "b154": true,
  "b155": true,
  "b156": false,
  "b157": true,
  "b158": false,
  "b159": false,
  "b160": false,
  "b161": false,
  "b162": false,
  "b163": false,
  "b164": false,
  "b165": false,
  "b166": false,
  "b167": false,
  "b168": false,
  "b169": false,
  "b170": false,
  "b171": false,
  "b172": false,
  "b173": true,
  "b174": true,
  "b175": true,
  "b176": false,
  "b177": false,
  "b178": false,
  "b179": false,
  "b180": false,
  "b181": true,
  "b182": false,
  "b183": false,
  "b184": false,
  "b185": true,
  "b186": false,
  "b187": false,
  "b188": true,
  "b189": false,
  "b190": false,
  "b191": false,
  "b192": false,
  "b193": false,
  "b194": false,
  "b195": false,
  "b196": false,
  "b197": false,
  "b198": false,
  "b199": false,
  "b200": true,
  "b201": false,
  "b202": false,
  "b203": false,
  "b204": false,
  "b205": false,
  "b206": false,
  "b207": false,
  "b208": true,
  "b209": false,
  "b210": false,
  "b211": false,
  "b212": false,
  "b213": false,
  "b214": true,
  "b215": false,
  "b216": false,
  "b217": false,
  "b218": true,
  "b219": false,
  "b220": true,
  "b221": false,
  "b222": false,
  "b223": true,
  "b224": false,
  "b225": false,
  "b226": false,
  "b227": false,
  "b228": false,
  "b229": false,
  "b230": false,
  "b231": false,
  "b232": true,
  "b233": false,
  "b234": false,
  "b235": false,
  "b236": false,
  "b237": false,
  "b238": false,
  "b239": false,
  "b240": false,
  "b241": false,
  "b242": false,
  "b243": false,
  "b244": false,
  "b245": false,
  "b246": false,
  "b247": false,
  "b248": true,
  "b249": false,
  "b250": false,
  "b251": false,
  "b252": false,
  "b253": false
}
```
