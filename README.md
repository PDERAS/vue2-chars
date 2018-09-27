# Vue Chars
A vue.js plugin that gets takes a string of tokens and produces that many single input boxes.
The value, however, will be one connected string (e.i. Not a bunch of single characters seperated)

## How to initialize Vue Chars
Vue chars is built as a vue plugin to allow for use throughout the application.

```javascript
import Chars from "@pderas/vue2-chars";

Vue.use(Chars);
```

## Usage
#### Creation
Vue chars is easily created, but at mininum requires a [mask](#masks). Having said that, Vue chars works best with v-model.
```HTML
<chars mask="####" v-model="myVar"></chars>
```

## Properties
| Property  | Required | Type                 | Default                 | Description                                   |
|-----------|----------|----------------------|:-----------------------:|-----------------------------------------------|
| mask      | true     | Number               | n/a                     | The mask for the input                        |
| value     | false    | Number&#124;String   | 0                       | Value for the input, can be used with v-model |


#### Masks
| Mask  | Description                                            |
|:-----:|--------------------------------------------------------|
| #     |  Number character only                                 |
| X     |  Alphanumeric character only                           |
| S     |  Alphabetic character only                             |
| A     |  Alphabetic character only (will convert to uppercase) |
| a     |  Alphabetic character only (will conver to lowercase)  |

## License
This project is covered under the MIT License. Feel free to use it wherever you like.