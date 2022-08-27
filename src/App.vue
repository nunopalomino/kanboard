<template>
  <div id="app">
    <Header />
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">Backlog</h2>
        <Container group-name="kanboard" @drag-start="handleDragStart('backlog', $event)"
          @drop="handleDrop('backlog', $event)" :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }">
          <Draggable v-for="card in cards.backlog" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
        <h3 class="add-title">+ Add card</h3>
      </div>
      <div class="lane">
        <h2 class="lane-title">Development</h2>
        <Container group-name="kanboard" @drag-start="handleDragStart('dev', $event)" @drop="handleDrop('dev', $event)"
          :get-child-payload="getChildPayload" :drop-placeholder="{ className: 'placeholder' }">
          <Draggable v-for="card in cards.dev" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
        <h3 class="add-title">+ Add card</h3>
      </div>
      <div class="lane">
        <h2 class="lane-title">Debug</h2>
        <Container group-name="kanboard" @drag-start="handleDragStart('debug', $event)"
          @drop="handleDrop('debug', $event)" :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }">
          <Draggable v-for="card in cards.debug" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
        <h3 class="add-title">+ Add card</h3>
      </div>
      <div class="lane">
        <h2 class="lane-title">Done</h2>
        <Container group-name="kanboard" @drag-start="handleDragStart('done', $event)"
          @drop="handleDrop('done', $event)" :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }">
          <Draggable v-for="card in cards.done" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
        <h3 class="add-title">+ Add card</h3>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header';
import Card from './components/Card';
import initialCards from './initialCards';
import { Container, Draggable } from "vue-smooth-dnd";


export default {
  name: 'App',
  components: {
    Header,
    Card,
    Container,
    Draggable
  },
  data: () => ({
    cards: {
      backlog: initialCards.backlog,
      dev: initialCards.dev,
      debug: initialCards.debug,
      done: [],
    },
    dragginCard: {
      lane: '',
      index: -1,
      cardData: {},
    }
  }),
  methods: {
    handleDragStart(lane, dragResult) {
      const { payload, isSource } = dragResult;
      if (isSource) {
        this.draggingCard = {
          lane,
          index: payload.index,
          cardData: {
            ...this.cards[lane][payload.index],
          }
        }
      }
    },
    handleDrop(lane, dropResult) {
      const { removedIndex, addedIndex } = dropResult;
      if (lane === this.draggingCard.lane & removedIndex === addedIndex) {
        return;
      }

      if (removedIndex !== null) {
        this.cards[lane].splice(removedIndex, 1);
      }

      if (addedIndex !== null) {
        this.cards[lane].splice(addedIndex, 0, this.draggingCard.cardData);
      }
    },
    getChildPayload(index) {
      return {
        index,

      };
    },
  },
};
</script>

<style>
.board {
  display: flex;
  justify-content: flex-start;
  margin: 6rem 0.8rem;
  align-items: flex-start;
}

.lane {
  background: var(--color-grey);
  width: 23rem;
  border-radius: 0.8rem;
  box-shadow: 0 .1rem .2rem 0 rgba(33, 33, 33, 0.1);
  margin: 0 0.8rem;
  padding: 0 0.7rem;
}

.lane-title {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
}

.placeholder {
  background: rgba(33, 33, 33, 0.08);
  border-radius: 0.4rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}

h2 {
  font-size: 1.9rem;
  font-weight: 400;
}

h3 {
  font-size: 1.5rem;
  font-weight: 400;
}

.add-title {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
  font-weight: 400;
  color: var(--color-light-grey);
  cursor: pointer;
}
</style>
