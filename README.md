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

#### place-holders: String
change the text in the placeholders.
##### example: place-holders="dates"

### example:
When click away or hit escape key the component fires event 'get-dates' that contains an object
with startDate and endDate keys holding values of start date and end date in utc format.

```HTML
  <vue-daterange-picker 
      double 
      start-date="06/10/2017" 
      end-date="06/10/2018" 
      place-holders="mm/dd/yyyy"
      @get-dates="getDates"/>
```
