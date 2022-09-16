<template>
  <v-container fluid class="fill-height">
    <v-row class="fill-height">
      <v-col cols="4">
        <v-data-table
          :headers="headers"
          :items="records"
          :items-per-page="5"
          class="elevation-1"
        ></v-data-table>
      </v-col>

      <v-col 
        cols="8" 
        class="d-flex justify-center align-center flex-column"
      >
        <div class="d-flex justify-center align-center flex-column">
          <StopWatch
            ref="stopWatch"
            :class="{'display-4': true, 'green--text': pressProgress == 100}"
            minutes
          />
          <v-progress-linear
            background-color="transparent"
            rounded
            :value="pressProgress"
            :color="pressProgress == 100 ? 'green' : 'white'"
          ></v-progress-linear>
        </div>
        <span v-show="!spaceDown && !$refs.stopWatch.isRunning" class="display-1 mt-4"> {{ scramble }} </span>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',

  data() {
    return {
      spaceDown: false,
      spaceDownInterval: null,
      pressProgress: 0,
      scramble: this.getScramble().join(' '),
      headers: [
        {
          text: 'Time',
          align: 'start',
          value: 'time',
        },
        {
          text: 'Ao5',
          value: 'ao5',
        },
        {
          text: 'Ao12',
          value: 'ao12',
        }
      ],

      records: [
        
      ],
    }
  },
  
  methods: {
    onSpaceDown() {
      this.spaceDown = true
      this.spaceDownInterval = setInterval(() => {
        if (this.pressProgress < 100) this.pressProgress = this.pressProgress + 10
      }, 100)
    },
    
    onSpaceUp() {
      this.spaceDown = false
      clearInterval(this.spaceDownInterval)
      if (this.pressProgress >= 100) {
        this.$refs.stopWatch.start()
      } else if (this.$refs.stopWatch.isRunning) {
        this.$refs.stopWatch.stop()
        this.records.push({
          time: this.$refs.stopWatch.time,
          ao5: '',
          ao12: '',
        })
        this.scramble = this.getScramble().join(' ')
      }
      this.pressProgress = 0
    },

    getScramble() {
      const directions = [
        ["D", "U"],
        ["L","R"],
        ["F","B"]
      ];

      const times = ["", "'", "2"];

      const random = (array, exclude) => {
        let n;
        do {
            n = Math.floor( Math.random() * array.length );
        } while(array[n] === exclude)
        return array[n];
      }

      const scramble = new Array(20);
      let direction;
      for(let i = 0; i < scramble.length; i++){
        direction = random(directions, direction);
        scramble[i] = random(direction) + random(times);
      }

      return scramble;
    }
  },

  mounted() {
    window.addEventListener('keypress', (e) => {
      if (e.code === 'Space') {
        if (!this.spaceDown) {
          this.onSpaceDown();
        }
      }
    });
    
    window.addEventListener('keyup', (e) => {
      if (e.code === 'Space') {
        this.onSpaceUp();
      }
    });
  },
}

</script>
