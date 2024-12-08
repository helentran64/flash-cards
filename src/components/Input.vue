<!-- Component to enter term and definition of a flash card -->
<template>
  <div class="inputContainer">
    <!-- Buttons to go back and forth between flash cards -->
    <div id="controls" v-if="addedFirstCard">
      <input type="button" value="previous" v-on:click="goBack" />
      <input type="button" value="next" v-on:click="goForward" />
    </div>
    <!-- Tracks which card you are currently on -->
    <div id="tracker" v-if="addedFirstCard">
      <p>{{ index + 1 }} / {{ getCardCount }}</p>
    </div>
    <div>
      <p id="addHeading">Add card</p>
    </div>
    <div class="inputField">
      <div class="submitSection">
        <label for="term">Term</label>
        <input type="text" id="term" ref="term" />
        <label for="def">Definition</label>
        <input type="text" id="def" ref="def" />
        <input
          type="button"
          value="submit"
          id="submit"
          v-on:click="submitInfo"
        />
      </div>
    </div>
  </div>

  <!-- Displays all the notes submitted -->
  <div class="listOfNotes" v-if="addedFirstCard">
    <p id="cardsHeading">Cards in this set ({{ getCardCount }})</p>
    <div class="note" v-for="(note, index) in flashCards">
      <p>Term: {{ note.term }}</p>
      <p>Definition: {{ note.definition }}</p>
      <input type="button" value="Delete Card" v-on:click="deleteCard(index)" />
    </div>
  </div>
</template>

<script>
export default {
  emits: ["info", "back", "forward", "emptyFlashCards"], // Initialize emits
  data() {
    return {
      index: 0,
      term: "",
      def: "",
      flashCards: [],
    };
  },
  computed: {
    // Get the number of cards in the flashCards array
    getCardCount() {
      return this.flashCards.length;
    },
    // Checks if there is at least one card in the flashCards array
    addedFirstCard() {
      if (this.getCardCount > 0) {
        return true;
      } else {
        return false;
      }
    },
  },
  mounted() {
    // When the page is loaded, the flash cards saved are displayed
    fetch("http://localhost:3000/flashCards")
      .then((res) => res.json())
      .then((data) => {
        this.flashCards = data;
        this.getData();
        this.$emit(
          "info",
          this.term,
          this.def,
          this.addedFirstCard,
          this.flashCards
        );
      })
      .catch((err) => console.log(err.message));
  },
  methods: {
    // Once user submits, their info will be sent to Home.vue (parent) so it can be sent to FlashCard.vue for display
    submitInfo() {
      this.term = this.$refs.term.value;
      this.def = this.$refs.def.value;
      // Post only unique terms to the database for display
      let duplicate = this.hasDuplicates({ term: this.term, def: this.def });
      if (!duplicate) {
        this.postData({ term: this.term, definition: this.def })
          .then(() => {
            this.$emit(
              "info",
              this.term,
              this.def,
              this.addedFirstCard,
              this.flashCards
            );
          })
          .catch((err) => {
            console.error("Error " + err);
          });
      }
      // Clear the input fields after submission
      this.$refs.term.value = "";
      this.$refs.def.value = "";
    },
    // Returns true if the flashCards array has a duplicate if the newNote
    hasDuplicates(newNote) {
      let current = newNote.term;
      for (let i = 0; i < this.flashCards.length; i++) {
        if (this.flashCards[i].term == current) {
          return true;
        }
      }
      return false;
    },
    goBack() {
      if (this.index > 0) {
        this.index -= 1;
        // Send information to Home.vue
        this.$emit("back", this.index);
      }
    },
    goForward() {
      if (this.index < this.getCardCount - 1) {
        this.index += 1;
        // Send information to Home.vue
        this.$emit("forward", this.index);
      }
    },
    // Post data to the database
    async postData(data) {
      try {
        const response = await fetch("http://localhost:3000/flashCards", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(data),
        });
        await this.getData();
      } catch (err) {
        console.error("Error posting data");
      }
    },
    // Fetch data from the database and store the output to flashCards array
    async getData() {
      try {
        const response = await fetch("http://localhost:3000/flashCards");
        const data = await response.json();
        this.flashCards = data;
      } catch (err) {
        console.error("Error fetching data " + err);
      }
    },
    // Delete entry from JSON database
    async deleteCard(index) {
      try {
        // Get current flash card id
        let currentId = this.flashCards[index].id;
        // Delete the JSON entry with the specified id
        await fetch("http://localhost:3000/flashCards/" + currentId, {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.flashCards),
        });
        // Remove the flash card at the specified index
        this.flashCards.splice(index, 1);

        // If flashCards array is empty, send message to Home.vue, then to FlashCard.vue
        if (this.getCardCount == 0) {
          this.$emit("emptyFlashCards", this.getCardCount);
        }
      } catch (err) {
        console.error(err.message);
      }
    },
  },
};
</script>

<style scoped>
p {
  color: white;
}
#controls {
  max-width: fit-content;
  margin-left: auto;
  margin-right: auto;
  padding: 10px;
}
input {
  padding: 5px;
  margin: 2px;
}
#tracker {
  max-width: fit-content;
  margin-left: auto;
  margin-right: auto;
  padding: 10px;
}
#addHeading {
  font-size: 20px;
}
.inputContainer {
  width: 600px;
  margin-left: auto;
  margin-right: auto;
}
.inputField {
  background-color: rgb(48, 44, 75);
  padding: 20px;
  border-radius: 10px;
  width: 560px;
  margin-left: auto;
  margin-right: auto;
}
label {
  padding: 5px;
  color: white;
}
#submit {
  margin: 5px;
}
.submitSection {
  max-width: fit-content;
  margin-right: auto;
  margin-left: auto;
}
.listOfNotes {
  width: 600px;
  margin-left: auto;
  margin-right: auto;
}
#cardsHeading {
  font-size: 20px;
}
.note {
  background-color: rgb(48, 44, 75);
  border-radius: 10px;
  width: 560px;
  padding: 20px;
  margin-right: auto;
  margin-left: auto;
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
