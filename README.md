# Luis Mocker [![CircleCI](https://circleci.com/gh/microsoftly/luis-mocker.svg?style=shield)](https://circleci.com/gh/microsoftly/luis-mocker)
Easy http mocking of luis.ai. Perfect for testing services that rely on Luis.ai by making them deterministic and avoiding pay per call requests.

This is a work in progress. PRs are welcome! 
# installation
```npm install -save luis-mocker```
# Classes
## LuisMocker
### ```constructor(luisModelUrl)```
### ```mock(utterance, responseBody)```
* mock utterance is the string that is expected.
* responseBody is the response sent back when that utterance is checked. Bodies can be easily built with the LuisResponseBuilder
## MockLuisResponseBuilder
follows a method chaining pattern, e.g.
```javascript
    new MockLuisResponseBuilder('I want to return my purchase')
        .addIntent('returns', 0.93)
        .addIntent('purchase', .3)
        .addIntent('none': .01)
        .build()
```
### ```constructor(query)```
### ```addIntent(intent, score)```
### ```addPrebuiltEntity(prebuiltEntity)```
* prebuiltEntity uses the PrebuiltEntity schema found in [luis-response-builder](github.com/microsoftly/luis-response-builder)
### ```addCustomEntity(customEntity)```
* prebuiltEntity uses the CustomEntity schema found in [luis-response-builder](github.com/microsoftly/luis-response-builder)
### ```build()```
returns an full luis.ai response object
# example use
```javascript
const 
import { expect } from 'chai';
import { DateTimeV2 } from 'luis-response-builder';
import * as rp from 'request-promise';
import { ILuisResponse } from './../src/ILuisResponse';
import { LuisMocker } from './../src/LuisMocker';
import { LuisResponseBuilder } from './../src/LuisResponseBuilder';
const cancelTacoBellResponse = new LuisResponseBuilder(cancelTacoBellReservationUtterance)
    .addCustomEntity({ startIndex: 0, endIndex: 4, type: RESTAURANT_NAME, entity: TACO_BELL, score: .98 })
    .addPrebuiltEntity(july15AmbiguousDateEntity)
    .addIntent(CANCEL_INTENT, .92)
    .addIntent(LOGIN_INTENT, .3)
    .addIntent(NONE_INTENT, .02)
    .build();
```