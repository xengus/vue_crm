<script lang="ts">
import Vue from 'vue';

import { mapActions, mapGetters } from 'vuex';

import HomeBillComponent from '../components/home/HomeBillComponent.vue';
import HomeCurrencyComponent from '../components/home/HomeCurrencyComponent.vue';
import LoaderComponent from '../../components/LoaderComponent.vue';

export default Vue.extend({
  name: 'HomePage',

  components: {
    HomeBillComponent,
    HomeCurrencyComponent,
    LoaderComponent,
  },

  data: () => ({
    loading: true,
  }),

  computed: {
    ...mapGetters(['currenciesGetter', 'currencyBaseGetter', 'infoBillGetter']),

    currency(): any {
      return this.currenciesGetter;
    },

    currencyBase(): any {
      return this.currencyBaseGetter;
    },

    bill(): any {
      return this.infoBillGetter;
    },
  },

  async mounted() {
    await this.currencyFetchAction();
    this.loading = false;
  },

  methods: {
    ...mapActions(['currencyFetchAction']),

    async refresh() {
      this.loading = true;
      await this.currencyFetchAction();
      this.loading = false;
    },
  },
});
</script>

<template>
  <div>
    <v-card-title>
      Счет ({{ bill | currencyFilter(currencyBase) }})
      <v-spacer></v-spacer>
      <v-btn color="secondary" @click="refresh">
        обновить
      </v-btn>
    </v-card-title>

    <LoaderComponent v-if="loading" />

    <div v-else-if="!loading" class="row">
      <HomeBillComponent :currency="currency" />
      <HomeCurrencyComponent :currency="currency" />
    </div>
  </div>
</template>
