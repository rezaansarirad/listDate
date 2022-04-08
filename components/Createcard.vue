<template>
  <v-row class="pa-10 mt-10">
    <v-col cols="12" lg="3" md="5" v-for="(item, i) in boostLists" :key="i">
      <div class="bg-card mt-10" :class="{ 'max-height': item.actionBoost }">
        <div
          class="bg-behind d-flex justify-center align-center"
          :class="{ 'bg-overlab': item.actionBoost }"
        >
          <v-icon
            v-show="item.actionBoost"
            large
            color="#fff"
            @click="item.actionBoost = !item.actionBoost"
            >mdi-plus</v-icon
          >
        </div>
        <div class="pa-4" v-if="!item.actionBoost">
          <div class="d-flex justify-space-between align-centern">
            <button
              class="btn-boost"
              @click="item.actionBoost = !item.actionBoost"
            >
              Boost
            </button>
            <v-btn icon>
              <v-icon color="error" @click="deleteBoost(i)">mdi-close</v-icon>
            </v-btn>
          </div>
          <div
            class="d-flex justify-space-between align-center mt-5 card-content"
          >
            <h3>Liking</h3>
            <span>100</span>
          </div>
        </div>
        <div v-else>
          <div class="d-flex justify-center align-center pa-3 mt-5">
            <button class="btn-create-boost" @click="CreateNewBoost(item)">
              Create Boost
            </button>
          </div>

          <h2 class="_title_sections">
            <span class="_txt">Date Period</span>
            <div class="_line"></div>
          </h2>
          <v-container>
            <v-form ref="dateForm" v-model="valid" lazy-validation>
              <p class="text-center">Pick Date Range:</p>
              <v-row class="d-flex justify-center align-center">
                <v-col cols="12" sm="6" md="5">
                  <v-menu
                    v-model="item.menu1"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        :rules="[rules.required]"
                        v-model="item.startDate"
                        placeholder="Start Date"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="item.startDate"
                      @input="menu1 = false"
                    ></v-date-picker>
                  </v-menu>
                </v-col>
                <v-spacer></v-spacer>
                <v-col cols="12" sm="6" md="2"> TO </v-col>
                <v-col cols="12" sm="6" md="5">
                  <v-menu
                    v-model="item.menu2"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        :rules="[rules.required]"
                        v-model="item.endDate"
                        placeholder="End Date"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="item.endDate"
                      @input="menu2 = false"
                    ></v-date-picker>
                  </v-menu>
                </v-col>
                <v-spacer></v-spacer>
              </v-row>
            </v-form>
          </v-container>
        </div>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      actionBoost: false,
      startDate: "",
      endDate: "",
      menu1: false,
      menu2: false,
      boostLists: [
        {
          actionBoost: false,
          startDate: "",
          endDate: "",
          menu1: false,
          menu2: false,
        },
      ],
      valid: true,
      rules: {
        required: (value) => !!value || "This value is required!",
      },
    };
  },
  methods: {
    CreateNewBoost(item) {
      this.boostLists.push({
        actionBoost: false,
        startDate: item.startDate,
        endDate: item.endDate,
        menu1: false,
        menu2: false,
      });
      item.actionBoost = false;

      localStorage.setItem("boostList", JSON.stringify(this.boostLists));
    },
    deleteBoost(index) {
      this.boostLists.splice(index, 1);
      localStorage.setItem("boostList", JSON.stringify(this.boostLists));
    },
  },
  created() {
    const list = localStorage.getItem("boostList");
    if (list) {
      this.boostLists = JSON.parse(list);
    }
  },
};
</script>

<style scoped>
.btn-boost {
  border-radius: 15px;
  background: #fff;
  border: 1px solid #cdd0dd;
  color: #ecedf3;
  padding: 3px 15px;
  text-transform: uppercase;
}
.bg-card {
  background: #fff;
  border: 1px solid #e3e3e3;
  border-radius: 8px;
  position: relative;
  z-index: 1;
  transition: all 0.4s ease-out;
  transition: 0.4s;
}

.bg-behind {
  background: #fff;
  border: 1px solid #e3e3e3;
  width: 95%;
  height: 10px;
  top: -10px;
  height: 10px;
  transform-origin: center top;
  position: absolute;
  left: 0;
  right: 0;
  margin: auto;
  z-index: -1;
  border-radius: 8px 8px 0 0;
  transition: top 0.4s ease-out;
}
.bg-overlab {
  top: -50px;
  z-index: -1;
  height: 50px;
  background: #515bce;
  transition: top 0.4s ease-out;
}
.max-height {
  height: 450px;
  transition: height 0.5s ease-in;
}
._title_sections {
  font-size: 28px;
  color: #000511;
  position: relative;
  text-align: center;
  margin: 30px auto;
  z-index: 2;
}
._title_sections ._txt {
  background: #fff;
  padding: 0 20px;
  font-size: 17px;
  color: #b7b7b7;
}
._title_sections ._line {
  position: absolute;
  right: 0;
  left: 0;
  height: 1px;
  border-top: 2px dashed #b2bac3;
  top: 50%;
  z-index: -1;
}

.btn-create-boost {
  font-size: 16px;
  font-weight: 500;
  background-color: #eaebf9;
  color: #787fd8;
  border-radius: 15px;
  padding: 3px 10px;
  text-transform: uppercase;
}
.card-content {
  text-transform: uppercase;
}
.card-content h3 {
  font-weight: 500;
}
.card-content span {
  font-size: 26px;
  font-weight: bold;
}
</style>
