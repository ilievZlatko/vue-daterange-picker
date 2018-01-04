# vue-daterange-picker

A vue component for picking range of dates.

# Install
```bash
npm install vue-daterange-picker --save
```

# import in project:
```javascript
  import VueDaterangePicker from 'vue-daterange-picker';
  
  components: {
    VueDaterangePicker
  }
```

# Dependencies:

#### vue-clickaway:  npm install vue-clickaway --save
#### momentJS: npm install moment --save

# Props:

Component expects properties:
#### start-date: String 
#### end-date: String
format 'MM/DD/YY'.
And then it will initiate callendar between these dates.

#### double: Boolean
display single callendar or double callendar.

#### format: String
changes the format of the inputs (default: 'MM/DD/YYYY')

#### title-format: String
changes the format of the calendar months (default: 'MMMM Y')

#### start-place-holder: String
changes the text in the start Date placeholder.
#### example: start-place-holder="01/01/2018"

#### end-place-holder: String
changes the text in the end Date placeholder.
#### example: end-place-holder="01/01/2018"

### example:
When click away or hit escape key the component fires event 'get-dates' that contains an object
with startDate and endDate keys holding values of start date and end date in utc format.

```HTML
  <vue-daterange-picker 
      double 
      start-date="06/10/2017" 
      end-date="06/10/2018" 
      start-place-holders="12/01/2017"
      end-place-holders="04/01/2018"
      @get-dates="getDates"/>
```
