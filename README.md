# vue-daterange-picker

A vue component for picking range of dates.

# Install
```bash
npm install vue-daterange-picker --save
npm install moment --save
npm install vue-clickaway --save
```

# In your main.js file:
```javascript
 import moment from 'moment';
 
 Vue.prototype.moment = moment;
```

# import in project:
```javascript
  import VueDaterangePicker from 'node_modules/vue-daterange-picker/vue-daterange-picker';
  
  components: {
    VueDaterangePicker
  }
```

# Dependencies:
You need to install:

#### vue-clickaway:  npm install vue-clickaway --save
#### momentJS: npm install moment --save

# Props:

Component expects properties:
#### start-date, end-date: String
utc, or any kind of date string.
And then it will initiate callendar between these dates.

#### double: Boolean
display single callendar or double callendar.

#### format: String
changes the format of the inputs (default: 'MM/DD/YYYY')

#### title-format: String
changes the format of the calendar months (default: 'MMMM Y')

### example:
When click away or hit escape key the component fires event 'get-dates' that contains an object
with startDate and endDate keys holding values of start date and end date in utc format.

```!DOCTYPE html
  <vue-daterange-picker double start-date="06/10/2017" end-date="06/10/2018" @get-dates="getDates"/>
```
vue daterange-picker component
