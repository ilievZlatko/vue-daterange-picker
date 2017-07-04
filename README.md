# vue-daterange-picker

A vue component for picking range of dates.

# install
```bash
pm i vue-daterange-picker --save
```

# dependencies
The vue-daterange-picker is using momentjs and vue-clickaway as dependencies.
They will be automatically installed with the package.

# usage:
Component expects properties:
start-date, end-date: utc, or any kind of date string.
And then it will initiate callendar between these dates.
double: display single callendar or double callendar.

# import in project:

```javascript
  import VueDaterangePicker from 'src/components/vue-daterange-picker';
```

### example:
```!DOCTYPE html
  <vue-daterange-picker double start-date="06/10/2017" end-date="06/10/2018"/>
```
vue daterange-picker component
