<template>
    <div id="mobipsum-app">

        <div class="bg-fade"></div>

        <div class="container">

            <h1>Mobipsum</h1>

            <div class="parameters">
                <label for="param_words">
                    Word / Sentence
                </label>
                <select id="param_words"
                        v-model="input.words">
                    <option v-for="i in 10" :value="i">{{ i }}</option>
                </select>
                <label for="param_sentences">
                    Sentence / Paragraph
                </label>
                <select id="param_sentences"
                        v-model="input.sentences">
                    <option v-for="i in 10" :value="i">{{ i }}</option>
                </select>
                <label for="param_paragraphs">
                    Paragraphs
                </label>
                <select id="param_paragraphs"
                        v-model="input.paragraphs">
                    <option v-for="i in 10" :value="i">{{ i }}</option>
                </select>
                <button @click="generate()">
                    Generate
                </button>
            </div>

            <div class="output">
                <p class="paragraph"
                   v-for="paragraph in output">
                    <span class="sentence"
                          v-for="sentence in paragraph">
                        <span class="word"
                              v-for="word in sentence"
                              :title="word.tip">
                            {{ word.phrase }}
                        </span>
                    </span>
                </p>
            </div>

        </div>

    </div>
</template>

<style lang="scss">

    html, body {
        margin: 0;
        padding: 0;
        height: 100%;
        min-height: 100%;
        color: #fff;
    }

    #mobipsum-app {
        background-image: url('./assets/bg.jpg');
        background-size: cover;
        background-repeat: no-repeat;
        background-position-x: center;
        background-position-y: top;
        min-height: 100%;

        .bg-fade {
            position: absolute;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 1) 80%);
        }

        .container {
            position: relative;

            h1 {
                margin: 0;
                padding: 24px 0;
                color: #eee;
                text-align: center;
                text-transform: uppercase;
            }

            .parameters {
                text-align: center;
                padding: 12px;
            }

            .output {
                padding: 12px 0;
                margin: 0 auto;
                text-align: justify;
                max-width: 680px;

                .paragraph {

                    .sentence {

                        .word {
                            &:hover {
                                text-decoration-line: underline;
                                text-decoration-style: dashed;
                                text-decoration-color: #fff;
                            }
                        }
                    }
                }
            }

        }

    }
</style>

<script>

  import fetch from './dictionary'

  export default {
    name: 'mobipsum-app',
    data() {
      return {
        input: {
          words: 10,
          sentences: 5,
          paragraphs: 3,
        },
        output: [],
      }
    },
    methods: {
      generate() {
        this.output = [];
        for(let p=0; p<this.input.paragraphs; p++) {
          let paragraph = [];
          for(let s=0; s<this.input.sentences; s++) {
            let sentence = [];
            for(let w=0; w<this.input.words; w++) {
              let i = fetch(1).pop();
              i.phrase = i.phrase.toLowerCase();
              sentence.push(i);
            }
            let first = sentence[0].phrase.toLowerCase();
            sentence[0].phrase = first.substr(0,1).toUpperCase() + first.substr(1);
            sentence[sentence.length-1].phrase+= '.';
            paragraph.push(sentence);
          }
          this.output.push(paragraph);
        }
      },
    }
  }
</script>
