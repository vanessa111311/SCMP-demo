<template>
  <div class="wrapper">
    <!-- Selected poll item (default is first item) -->
    <div class="pollItemSel">
      <div>
        <div class="pollTitleSel">{{this.PollsSel.Title}}</div>
        <div class="pollPublishedSel">PUBLISHED : {{this.PollsSel.PublishedDate | dateFormat}}</div>
        <div class="pollContentSel">
            <div class="pollAnswer" :class="{'multipieAnswer':this.PollsSel.AnswerOptions.length > 2}">           
              <button v-for="(answerItem,answerIndex) in this.PollsSel.AnswerOptions" :key="answerIndex" @click="Vote(answerItem)" class="pollAnswerOptions">
                {{answerItem.label | answerFormat}}
              </button>
            </div>
            <div class="pollPieChartWrapper">
              <div class="pollPieChart">
                <div class="voteTitle" v-if="this.PollsSel.TotalNumber === 0">Let's take a vote. and you will see the pie chart !</div>
                <GChart
                  v-if="this.PollsSel.TotalNumber>0"
                  type="PieChart"
                  :data="PiePoll"
                  :options="PiePollOptions"
                />
              </div>
            </div>

            <div class="pollTotalNumber">Total number of votes recorded: {{this.PollsSel.TotalNumber}}</div>
        </div>
      </div>
    </div>
    <!-- All poll items (except selected poll item) -->
    <div class="pollItemWrapper">
      <div  v-for="(item, index) in PollsData"  :key="index" @click="SelectedPollItem(index)">
        <div v-if="PollsSel.Title !== item.title" class="pollItem">
          <div class="pollItemPic">
            <img src="../assets/pieDemo.jpg">
          </div>
          <div class="pollItemContent">
            <div class="pollPublished">{{item.publishedDate | dateFormat('pollAll')}}</div>
            <div class="pollTitle">{{item.title}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import { GChart } from "vue-google-charts";

export default {
  components: {
    GChart
  },
  data() {
    return {
      PollsData:[],
      PollsSel:{
        Title: '',
        PublishedDate: '',
        AnswerOptions: [],
        TotalNumber: 0,
      },
      PiePoll: [
          ['Gender', 'Overall'],
          ['YES', 1],
          ['NO', 1]
      ],
      PiePollOptions: {
        legend:'none',
        width: 200,
        height: 200,
        position: 'initial',
        backgroundColor: '#accfe5',
        chartArea:{
          width:"90%",
          height:"90%"
          
        },
        colors: [
          "#f46403",
          "#003b6f",
          "#EF5350",
          "#AB47BC",
          "#42A5F5",
          "#009688",
          "#FFC107"
        ]
      }
    }
  },
  filters:{
    dateFormat:function (value, dse){
      let date = new Date(value);

       // All poll item
      if(dse === 'pollAll'){
        let monthNames = ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'];
        
        let newDateFormat =  `${date.getDate()}   
        ${monthNames[date.getMonth()+1]} ${date.getFullYear()}`; // getMonth() is start to 0, so have to plus 1

        return newDateFormat;

      // Selected poll item
      }else{
        let monthNames = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
        let dayNames = [ 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
        
        // Converting am/pm to 24 Hour
        let hours = date.getHours()
        let minutes = date.getMinutes()
        let ampm = hours >= 12 ? 'pm' : 'am'
        hours %= 12
        hours = hours || 12 // the hour '0' should be '12'
        minutes = ( minutes < 10 ) ? `0${minutes}` : minutes
        let strTime = `${hours}:${minutes}${ampm}`;

        let newDateFormat =  `${dayNames[date.getMonth()]}, ${date.getDate()}   
        ${monthNames[date.getMonth()+1]}, ${date.getFullYear()}, ${strTime}`;

        return newDateFormat;
     
     }
    },
    answerFormat:function (value){
      return value.toUpperCase();
    }
  },
  created() {
    this.GetPOllsData();
    this.SelectedPollItem(0); // Default is the first Item
  },
  methods:{
    GetPOllsData(){
      // Call the API to get poll list data
      // (In this case is use data that from pdf file).
      let data = {
        "polls": [
          {
            "id": 1,
            "title": "Is bitcoin worth the time and money that mining requires?",
            "publishedDate": 1516605447,
            "answer": {
              "type": "Single",
              "options": [
                {
                  "id": 1,
                  "label": "Yes"
                },
                {
                  "id": 2,
                  "label": "No"
                }
              ]
            }
          },
          {
            "id": 2,
            "title": "Should chatbots replace humans in customer service jobs?",
            "publishedDate": 1516000647,
            "answer": {
              "type": "Single",
              "options": [
                {
                  "id": 3,
                  "label": "Yes"
                },
                {
                  "id": 4,
                  "label": "No"
                }
              ]
            }
          },
          {
            "id": 3,
            "title": "How are we feeling about 2018?",
            "publishedDate": 1515568647,
            "answer": {
              "type": "Single",
              "options": [
                {
                  "id": 5,
                  "label": "Hopeful"
                },
                {
                  "id": 6,
                  "label": "Doubtful"
                }
              ]
            }
          },
          {
            "id": 4,
            "title": "Which country/region have you ever visited? (Select all that applies)",
            "publishedDate": 1515568647,
            "answer": {
              "type": "Multi",
              "options": [
                {
                  "id": 7,
                  "label": "Hong Kong"
                },
                {
                  "id": 8,
                  "label": "China"
                },
                {
                  "id": 9,
                  "label": "Australia"
                },
                {
                  "id": 10,
                  "label": "Thailand"
                },
                {
                  "id": 11,
                  "label": "Korea"
                },
                {
                  "id": 12,
                  "label": "Japan"
                }
              ]
            }
          },
          {
            "id": 5,
            "title": "Will new benefits encourage you to study or work in mainland?",
            "publishedDate": 1515309447,
            "answer": {
              "type": "Single",
              "options": [
                {
                  "id": 13,
                  "label": "Yes"
                },
                {
                  "id": 14,
                  "label": "No"
                }
              ]
            }
          }
        ]
      };
      
      this.PollsData = data.polls;

    },
    SelectedPollItem(index){
      this.PollsSel.Title = this.PollsData[index].title;
      this.PollsSel.PublishedDate = this.PollsData[index].publishedDate;
      this.PollsSel.AnswerOptions = this.PollsData[index].answer.options;
      
      this.PollsSel.TotalNumber = 0;
      let totalNumber = 0;

      // Get the original total number
      this.PollsData[index].answer.options.forEach(function(item){
        if(item.number){
          totalNumber += item.number;

        }
      })
      this.PollsSel.TotalNumber = totalNumber;
      

      // Renew data for pie chart
      this.RefreshPieChart();

    },
    Vote(answer){
      // Total number of votes recorded:
      this.PollsSel.TotalNumber ++;

      // Store answer number in object, let pie chat to use.
      if(!answer.number){ // Not yet voted 
        answer.number = 1;
      
      
      }else{ // Otherwise plus 1
        answer.number = answer.number +1;
      }

      // Renew data for pie chart
      this.RefreshPieChart();

    },
    RefreshPieChart(){
      // Renew data for pie chart
      this.PiePoll = [['Gender', 'Overall']];
      this.PollsSel.AnswerOptions.forEach(item =>{
        let pieItem = [];

        pieItem.push(item.label);
        pieItem.push(item.number?item.number:0);
        this.PiePoll.push(pieItem);
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper{
  max-width: 1200px;
  margin: 0 auto;
}
.pollItemWrapper{
  position: relative;
}
.pollItem{
  float: left;
  width: calc(50% - 2px);
  height: 100px;
  padding-top: 20px;
  border: 1px solid #E0E0E0;
  cursor: pointer;
}
.pollPieChart{
  float: right;
  margin: 40px;
}
.pollItemPic Img{
  width: 80px;
  height: 75px;
  float: left;
  margin-right: 10px;
}
.pollItemContent{
  height: 100px;
  text-align: left;
}
.pollTitle{
  font-size: 18px;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  display: -webkit-box;
}
.pollPublished{
  font-size: 14px;
  color: #003b6f;;
  font-weight: bold;
}
/* Selected  */
.pollItemSel{
  cursor: pointer;
  width: auto;
}
.pollTitleSel{
  text-align: left;
  font-size: 30px;
  border-bottom: 1px solid #E0E0E0;
  padding-bottom: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  display: -webkit-box;
  max-height: 120px;
}
.pollPublishedSel{
  font-size: 12px;
  margin: 10px 0;
  text-align: right;
  color: #9E9E9E;
  font-weight: bold;
}
.pollContentSel{
  position: relative;
  background: #accfe5;
  height: 400px;
  overflow: hidden;
}
.pollAnswer {
  margin: 50px;
  position: absolute;
  display: grid;
  min-width: 30px;
}
.pollAnswerOptions{
  margin: 5px 0;
  font-weight: bold;
  min-width: 50px;
  height: 30px;
  color: #ffffff;
  background-color: #003b6f;
  border: 0;
  float: left;
  cursor: pointer;
}
.pollAnswerOptions:nth-child(1){
  background-color: #f46403;
}
.pollAnswerOptions:nth-child(2){
  background-color: #003b6f;
}
.pollAnswerOptions:nth-child(3){
  background-color: #EF5350;
}
.pollAnswerOptions:nth-child(4){
  background-color: #AB47BC;
}
.pollAnswerOptions:nth-child(5){
  background-color: #42A5F5;
}
.pollAnswerOptions:nth-child(6){
  background-color: #009688;
}
.pollAnswerOptions:nth-child(6){
  background-color: #FFC107;
}

.pollTotalNumber{
  margin: 50px;
  position: absolute;
  bottom: 0;
}
.voteTitle{
  margin: 100px auto;
}

@media (max-width: 480px) {
  .pollPublishedSel{
    display: none;
  }
  .pollTitleSel{
    position: absolute;
    margin: 10px;
    z-index: 1;
    border-bottom: none;
    font-size: 24px;
  }
  
  .pollAnswer{
    margin: 155px 10px 0 10px;
    z-index: 1;
  }
  .pollContentSel{
    height: 550px;
  }
  .pollPieChart{
    width: 100%;
    margin: 230px 0 0 0;
  }
  .pollPieChartWrapper{
    display: inline-block;
    margin-top: 25px;
  }
  .pollTotalNumber{
    width: 100%;
    margin: 20px auto;
  }
  .pollItem{
    width: calc(100% - 2px);
  }
  .multipieAnswer{
    display: block;
  }
  .multipieAnswer .pollAnswerOptions{
    margin: 2px 2px;
  }
}

</style>
