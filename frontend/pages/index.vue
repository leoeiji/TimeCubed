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

      <v-col cols="8">
        <StopWatch
          ref="stopWatch"
          class="digits"
          minutes
          :hours="false"
        />
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
      spaceDownTime: null,
      headers: [
        {
          text: 'Dessert (100g serving)',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Calories', value: 'calories' },
        { text: 'Fat (g)', value: 'fat' },
        { text: 'Carbs (g)', value: 'carbs' },
        { text: 'Protein (g)', value: 'protein' },
        { text: 'Iron (%)', value: 'iron' },
      ],

      records: [
        
      ],
    }
  },
  
  methods: {
    onSpaceDown() {
      this.spaceDown = true;
      this.spaceDownTime = performance.now();
    },
    
    onSpaceUp() {
      this.spaceDown = false
      if (performance.now() - this.spaceDownTime > 1000) {
        this.$refs.stopWatch.start();
      } else {
        if (this.$refs.stopWatch.isRunning) {
          this.$refs.stopWatch.stop();
        }
      }      
    },
  },

  mounted() {
    window.addEventListener('keydown', (e) => {
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
