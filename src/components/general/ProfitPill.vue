<template>
  <div
    class="d-flex justify-content-center align-items-center profit-pill px-2"
    :class="isProfitable ? 'profit-pill-profit' : ''"
    :title="profitDesc"
  >
    {{ profitRatio !== undefined ? formatPercent(profitRatio, 2) : '' }}
    <span
      v-if="profitString"
      class="ml-1"
      :class="profitRatio ? 'small' : ''"
      :title="stakeCurrency"
      >{{ profitString }}</span
    >
  </div>
</template>

<script lang="ts">
import { formatPercent, formatPrice, timestampms } from '@/shared/formatters';
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({})
export default class ProfitPill extends Vue {
  @Prop({ required: false, default: undefined, type: Number }) profitRatio?: number;

  @Prop({ required: false, default: undefined, type: Number }) profitAbs?: number;

  @Prop({ required: true, type: String }) stakeCurrency!: string;

  @Prop({ required: false, default: '', type: String }) profitDesc!: string;

  formatPercent = formatPercent;

  timestampms = timestampms;

  formatPrice = formatPrice;

  get isProfitable() {
    return (
      (this.profitRatio !== undefined && this.profitRatio > 0) ||
      (this.profitRatio === undefined && this.profitAbs !== undefined && this.profitAbs > 0)
    );
  }

  get profitString(): string {
    if (this.profitRatio !== undefined && this.profitAbs !== undefined) {
      return `(${formatPrice(this.profitAbs, 3)})`;
    }
    return '';
  }
}
</script>

<style scoped lang="scss">
.profit-pill {
  background: $color-loss;
  border-radius: 6px;
}
.profit-pill-profit {
  background: $color-profit;
}
</style>
