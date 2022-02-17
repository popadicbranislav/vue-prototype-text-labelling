<script setup lang="ts">
import { reactive } from "vue";

interface MessageObject {
  text: string;
  entities: {
    id: string;
    name: string;
    start: number;
    end: number;
  }[];
}

const props = defineProps<{ value: MessageObject }>();

const { text, entities } = props.value;

const textContent = reactive(
  (() => {
    // We add the original index to entities for onChange and onDelete methods, then we sort them by order of appearance.
    const sortedEntities = entities
      ? entities
          .map((entity, index) => ({ ...entity, index }))
          .sort((a, b) => {
            if (a.start !== undefined && b.start !== undefined) {
              return a.start - b.start;
            }
            return 0;
          })
      : [];
    const newTextContent = []; // an ordered list of the utterance cut down into text and entities.

    // if there is no text we can just get the sorted entities.
    if (!text) {
      sortedEntities.forEach((entity) => {
        newTextContent.push({
          ...entity,
          type: "entity",
        });
      });
      // If there is a text, we get text elements and entities, sorted in order of appearance.
    } else {
      const currentText: any = {
        type: "text",
      };

      const addChar = (index: number) => {
        const tempText = currentText.text;
        currentText.text = tempText
          ? tempText.concat(text.charAt(index))
          : text.charAt(index);
        if (!tempText) {
          currentText.start = index;
        }
      };

      for (let i = 0; i < text.length; i += 1) {
        // if (textSelection && i === textSelection.start) {
        //   i = textSelection.end;
        //   if (currentText.text) newTextContent.push({ ...currentText });
        //   delete currentText.text;
        //   newTextContent.push({ ...textSelection, type: "selection" });
        // }
        if (sortedEntities[0] && sortedEntities[0].start === i) {
          i = sortedEntities[0].end - 1;
          if (currentText.text) {
            newTextContent.push({ ...currentText });
            delete currentText.text;
          }
          newTextContent.push({
            ...sortedEntities[0],
            type: "entity",
          });
          sortedEntities.shift();
        } else {
          addChar(i);
        }
        if (i === text.length - 1) {
          if (currentText.text) newTextContent.push({ ...currentText });
        }
      }
    }
    return newTextContent;
  })()
);

// const htmlContent = `<span>${value.labels[0].text.substring(0,5)}</span>`
</script>

<template>
  <div class="message-base">
    <div class="message-base__content">
      {{ value.text }}
      <span class="entity">text</span>
    </div>

    <div class="message-base__actions">
      <button class="message-base__button">edit</button>
      <button class="message-base__button">delete</button>
    </div>
  </div>

  <hr />

  <h4>textContent:</h4>

  <div class="utterance">
    <template v-for="element of textContent">
      <span v-if="element.type == 'text'">{{ element.text }}</span>
      <span
        v-if="element.type == 'entity'"
        class="entity-class"
        :data-label="element.name"
        >{{ value.text.slice(element.start, element.end) }}

        <div class="entity-class__label">
          {{ element.name }}
        </div></span
      >
    </template>
  </div>

  <hr />

  <h4>labels:</h4>

  <div v-for="label in value.entities">
    {{ label }}
  </div>
</template>

<style scoped>
.message-base {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1em;

  padding: 0.5em;
  border-block: 0.05em solid #00000033;

  font-size: 1rem;
}

.message-base__actions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.message-base__button {
  aspect-ratio: 1/1;
  background: 0;
  border: 0;

  font-size: 0.8rem;
  font-weight: 900;
  text-transform: uppercase;
}

.utterance {
  /* display: inline-flex; */

  margin: 1rem;
}
.entity-class {
  position: relative;
  --entity-color: green;

  padding-inline-start: 0.2em;
  background: var(--entity-color);

  color: white;

  cursor: pointer;
}

.entity-class:hover {
  --entity-color: darkgreen;
}

.entity-class__label {
  position: absolute;

  background: var(--entity-color);
  width: 100%;

  font-size: 0.6em;
  text-align: center;

  --entity-border-radius: 0.3rem;

  border-bottom-right-radius: var(--entity-border-radius);
  border-bottom-left-radius: var(--entity-border-radius);

  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
