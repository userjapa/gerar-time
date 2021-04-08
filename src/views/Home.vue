<template>
  <div class="home container">
    <form class="form flex container column" @submit.prevent="gerarTime(names, $event.target)">
      <h4>Digite os nomes separados por parágrafo (Nomes repetidos serão removidos)</h4>
      <textarea
        name="name"
        rows="8"
        cols="80"
        v-model="names"
        :placeholder="placeholder"
        required
      />
      <input
        type="number"
        :min="2"
        :max="20"
        v-model="members"
        required
      >
      <button
        type="submit"
        name="button"
      >
        Gerar Time
      </button>
    </form>
    <div class="time flex container column">
      <h4>Times</h4>
      <div class="container wrap justify-start">
        <div class="time-box" v-for="(time, index) in times" :key="index">
          <div class="">
            <span>{{ time.name }}</span>
            <ul>
              <li v-for="(name, i) in time.players" :key="`name-${index}-${i}`">{{ name }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      names: '',
      times: [],
      members: 0,
      placeholder: `Exemplo:\nJeremias\nTobias\nFarias\nJuca\nTucano\nBob\nMarli\nJosueldo\n(Nomes repetidos serão removidos)`
    }
  },
  methods: {
    gerarTime (names, form) {
      if (!form.checkValidity())
        return form.reportValidity()

      this.times = []

      const namesArray = names.split('\n')

      const sorted = [ ...new Set(this.sortArray(namesArray).filter(n => !!n)) ]

      const members = parseInt(this.members)

      const size = sorted.length / members

      for (let x = 0; x < size; x++) {
        const team = sorted.splice(0, members)

        if (team.length == members) {
          this.times.push({
            name: `Time ${x + 1}`,
            players: team
          })
        } else {
          const remainder = members - team.length

          for (let x = 0; x < remainder; x++) {
            team.push('')
          }

          this.times.push({
            name: 'Reserva',
            players: team
          })
        }

      }
    },
    sortArray (array) {
      let currentIndex = array.length, temporaryValue, randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {

        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    }
  }
}
</script>

<style lang="scss">
@import '../assets/scss/variables';

.home {
  width: 100%;
  flex: 1;

  h4 {
    text-align: center;
    border-bottom: 1px solid $color-gray;
    padding-bottom: 4px;
  }

  .time {
    border-left: 1px solid $color-gray;
    padding: 5px;
    width: calc(50% - 10px);

    .time-box {
      font-weight: 700;
      margin: 5px;
      padding: 5px;
      width: 153px;
      text-align: start;
      border: 1px solid $color-gray;
      border-radius: 15px;
      span {
        margin-left: 10px;
      }
      ul {
        margin: 0;
        margin-top: 5px;
        padding-left: 20px;
      }
    }
  }

  form.form {
    padding: 5px;
    width: calc(50% - 10px);
    border-right: 1px solid $color-gray;

    input,
    textarea,
    button {
      border-radius: 7px;
      padding: 5px;
      font-size: 16px;
      margin: 5px;
      &:active,
      &:focus {
        outline: none;
      }
    }

    textarea {
      resize: vertical;
      height: 40%;
      max-width: calc(50vw - 20px);
      background-color: transparent;
      border: 1px solid $color-gray;
    }

    input {
      background-color: transparent;
      border: 1px solid $color-gray;
    }

    button {
      color: $color-gray;
      padding: 5px 15px;
      font-weight: 700;
      background-color: transparent;
      border: 2px solid $color-gray;
      transition: border-color .2s, background-color .2s, color .2s;

      &:hover {
        cursor: pointer;
        border-color: darken($color-gray, 5);
        background-color: darken($color-secondary, 2);
      }

      &:active,
      &:focus {
        background-color: darken($color-secondary, 5);
        color: $color-dark;
      }
    }
  }
}

.container {
  display: flex;
  &.column {
    flex-direction: column;
  }

  .flex {
    flex: 1;
  }

  .wrap {
    flex-wrap: wrap;
  }

  .justify-start {
    justify-content: flex-start;
  }
}

@media(max-width: 768px) {
  .home {
    flex-direction: column;

    form.form {
      width: calc(100% - 10px);
      border-bottom: 1px solid $color-gray;
      border-right: none;

      textarea {
        max-width: calc(100% - 10px);
        flex: 1;
      }
    }

    .time {
      width: calc(100% - 10px);
      border-top: 1px solid $color-gray;
      border-left: none;
    }

  }
}
</style>
