# vue-daterange-picker

A vue component for picking range of dates.

# install
```bash
npm install vue-daterange-picker --save
npm install moment --save
npm install vue-clickaway --save
```

# in your main.js file
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

# dependencies
The vue-daterange-picker is using momentjs and vue-clickaway as dependencies.
They will be automatically installed with the package.

# usage:
Component expects properties:
start-date, end-date: utc, or any kind of date string.
And then it will initiate callendar between these dates.
double: display single callendar or double callendar.
format: changes the format of the inputs (default: 'MM/DD/YYYY')
title-format: changes the format of the calendar months (default: 'MMMM Y')

### example:
```!DOCTYPE html
  <vue-daterange-picker double start-date="06/10/2017" end-date="06/10/2018"/>
```
vue daterange-picker component
