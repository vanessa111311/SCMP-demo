# This is how I approached this project:
1. Display two block :selected poll item and others poll items.

(1). First step: Call the API to get poll list data(In this case is use data that from pdf file).

(2). Write HTML to show selected poll item:
    <1>. Write SelectedPollItem() and set the dynamic value for HTML.
    <2>. Default is the first item in poll List connect with vue-model.
    <3>. Every time, when user click others poll items, it will will triiger SelectedPollItem() again, and change the value for selected poll item.

(3). Write HTML to show others poll items: Use v-for(for loop) to show all poll items (except selected poll item) 

2. Format publishedDate: 
Use filters to show the different date format. EX:
(1). selected poll item : Sunday, 18 February, 1970, 9:16pm

(2). others poll items : Sunday, 18 FEB 1970

3. Store poll result : Total number and answer:
(1). Write Vote(): Every time, When User click answer button will triiger.
    <1>. Total number: plus 1 in total number(this.PollsSel.TotalNumber).
    <2>. Store answer number in object, let pie char to use.

4. Chart will be refresh upon submission:
(1). Use vue-google-charts to draw pie chart.
(2). Write VoRefreshPieChartte(): Use answer number which is store before to renew the data for pie chart.
(3). Not yet voted:  show the word "Let's take a vote. and you will see the pie chart!"

5. Responsive (Mobile / Desktop):
(1). Defining Breakpoints in this case is 480px: @media (max-width: 480px);
(2). Avoid text overflow (Use text-overflow: ellipsis).
(3). Only for upper 320px width (width 320px for the smallest size).


## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
