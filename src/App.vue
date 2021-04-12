<template>
  <div id="app" class="hello" :style="{'background-image':'url(https://media.contentapi.ea.com/content/www-easports/en_US/fifa/ultimate-team/news/2017/fut-18-path-to-glory-faq/_jcr_content/headerImages/image.img.png)'}">
<h1>Was the referee call correct? Vote below !</h1> 
	<h1>(This is a Serverless Voting App)</h1>
	<h4>Click to vote</h4>
	<h4>You can vote as many times as you like</h4>
<b-row align-h="center" class="mt-5">
      <b-card-group deck>
        <b-card bg-variant="success" text-variant="white" header="Vote Yes" class="text-center" footer-tag="footer">
          <b-card-text>Yes. The call was correct.</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('yes')">Button</b-button>
          <em slot="footer">{{ votesYes }} voted</em>
        </b-card>
<b-card bg-variant="danger" text-variant="white" header="Vote No" class="text-center" footer-tag="footer">
          <b-card-text>No. The referee is wrong!</b-card-text>
          <b-button size="lg" variant="primary" @click="vote('no')">Button</b-button>
          <em slot="footer">{{ votesNo }} voted</em>
        </b-card>
      </b-card-group>
    </b-row>
    <b-row align-h="center" class="mt-5">
      <p>My github repo:  <a href="https://github.com/Minhbui7/">@minhbui7</a>.</p>
    </b-row>
  </div>  
</template>

<script>
import { API } from 'aws-amplify'
export default {
  name: 'app',
  data() {
    return {
      apiName: 'pollCounterAPI',
      votesYes: 0,
      votesNo: 0
    }
  },
  methods: {
    vote: async function (vote) {
      const init = {
        queryStringParameters: {
          vote
        }
      }
      const response = await API.post(this.apiName, '/votes', init)
      if (vote === 'yes') this.votesYes = response.data.Attributes.votesYes
      if (vote === 'no') this.votesNo = response.data.Attributes.votesNo
    },
    updateVotes: async function () {
      const response = await API.get(this.apiName, '/votes/poll-001')
      this.votesNo = response[0].votesNo
      this.votesYes = response[0].votesYes    
    }
  },
  created () {
    this.updateVotes()
    setInterval(this.updateVotes, 3000)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
