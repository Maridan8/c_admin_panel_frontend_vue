<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-row justify="center">
      <v-col cols="12" md="11">
        <v-card-title color="green lighten-1" class="px-5 py-1">

          <v-row>
            <v-col cols="12" md="4">
              <v-select
                v-model="contenant"
                :items="getContenants"
                :rules="[v => !!v || 'Contenant is required']"
                item-text="label"
                item-value="code"
                label="القارة"
                required
              ></v-select>
            </v-col>

            <v-col cols="12" md="6">
              <v-select
                v-model="id"
                :items="getCountries"
                :rules="[v => !!v || 'Country is required']"
                item-text="countryName"
                item-value="id"
                label="الدولة"
                required
              ></v-select>
            </v-col>

            <v-col cols="12" md="2" class="text-center">
              <v-tooltip top>
                <template v-slot:activator="{ on }">
                  <v-card-actions>

                    <v-btn fab x-small color="green lighten-1" @click="sendData" :disabled="!valid" v-on="on" >
                      <v-icon light>mdi-plus</v-icon>
                    </v-btn>
                    
                  </v-card-actions>
                </template>
                <span>إضافة</span>
             </v-tooltip>
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12" md="12">
              <v-simple-table dense>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-right">القارة</th>
                      <th class="text-right">الدولة</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in getAptCountries" :key="item.name & item.countryName">
                      <td>{{ item.contenant }}</td>
                      <td>{{ item.countryName }}</td>
                      <td>
                        <v-btn class="mx-2" v-on="on" fab x-small @click="close(item)">
                          <v-icon dark>mdi-delete</v-icon>
                        </v-btn>
                      </td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </v-col>
          </v-row>
        </v-card-title>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
export default {
  components: {},
  data() {
    return {
      valid: true,
      select: null,
      contenant: "",
      contenantName: {},
      id: 0,
      country: {
        countryName: "",
        id: 0,
        contenant: "Asia",
        status: { Id: 1, StatusString: "Active" }
      },
      countries: [],
      myheaders: [
        { text: "القارة", value: "contenant" },
        { text: "الدولة", value: "countryName" },
        { text: "", value: "Actions" }
      ]
    };

  },

  computed: {
    ...mapGetters("country", ["getCountries", "getContenants"]),
    ...mapGetters("apt", ["getAptCountries"])
    //countries: this.getAptCountries,
  },
  watch: {
    contenant: function() {
      this.changeContenant(this.contenant);
    },
    id: function() {
      this.country = this.getCountries.find(con => con.id == this.id);
    }
  },
  methods: {
    ...mapActions("apt", ["removeCountry", "AddCountry"]),
    ...mapActions("country", ["changeContenant"]),
    close(item) {
      this.removeCountry(item.Id);
    },

    sendData() {
      this.AddCountry(
        Object.assign(
          {},
          this.getCountries.find(con => con.id == this.country.id)
        )
      );
    }
  },
  mounted() {}
};
</script>
