<template lang="html">
  <section class="balance-panel col col-xs-12 col-sm-12 col-md-6">
    <h2 class="title">Your Balance</h2>
    <div class="content">
      <div class="row">
        <div class="col col-xs-6">
          <p>Current Balance</p>
          <h2 class="mb0 blue unit-numbers">
            <b>{{ balance }}</b>
          </h2>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { mapState } from 'vuex';
import { getSum, formatAmount } from '@/utils';

export default {
  name: 'balance-panel',

  computed: {
    ...mapState({
      credits: state => state.billing.credits,
      debits: state => state.billing.debits
    }),

    balance () {
      const debitSum = getSum(this.debits, 'amount');
      const promoCreditSum = getSum(this.credits, 'promo_amount');
      const paidCreditSum = getSum(this.credits, 'paid_amount');
      const creditSum = paidCreditSum + promoCreditSum;
      const freeCredit = 167;
      const balance = -creditSum + debitSum - Math.min(debitSum, freeCredit);

      return formatAmount(balance);
    }
  }
};
</script>

<style lang="scss">
</style>
