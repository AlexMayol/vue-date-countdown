<template>
  <section v-if="loaded">
    <h2 v-if="!expired">{{ text }}</h2>
    <h2 v-else>{{ end_text }}</h2>
    <span>
      {{ displayDays }}
    </span>
    :
    <span>
      {{ displayHours }}
    </span>
    :
    <span>
      {{ displayMinutes }}
    </span>
    :
    <span>
      {{ displaySeconds }}
    </span>
  </section>
</template>
<script>
export default {
  props: {
    text: {
      type: String,
      default: "",
    },
    end_text: {
      type: String,
      default: "",
    },
    year: {
      type: Number,
      default: new Date().getFullYear(),
    },
    month: {
      type: Number,
      required: true,
    },
    day: {
      type: Number,
      required: true,
    },
    hour: {
      type: Number,
      default: 23,
    },
    minute: {
      type: Number,
      default: 59,
    },
    second: {
      type: Number,
      default: 59,
    },
  },
  data: () => ({
    loaded: false,
    expired: false,
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
  }),
  mounted() {
    this.showRemaining();
  },
  computed: {
    _seconds: () => 1000,
    _minutes() {
      return this._seconds * 60;
    },
    _hours() {
      return this._minutes * 60;
    },
    _days() {
      return this._hours * 24;
    },
    endDate() {
      return new Date(this.year, this.month, this.day, this.hour, this.minute, this.second);
    },
  },
  methods: {
    formatNum: (num) => (num < 10 ? "0" + num : num),
    showRemaining() {
      const timer = setInterval(() => {
        const now = new Date();
        const distance = this.endDate.getTime() - now.getTime();

        if (distance < 0) {
          clearInterval(timer);
          this.loaded = true;
          this.expired = true;
          return;
        }

        const days = Math.floor(distance / this._days);
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);

        this.displayMinutes = this.formatNum(minutes);
        this.displaySeconds = this.formatNum(seconds);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        this.loaded = true;
      }, 1000);
    },
  },
};
</script>
