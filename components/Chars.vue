<template>
    <div class="boxed-input">
        <input v-for="(char, idx) in mask"
               :ref="'box-input-' + idx"
               :id="'box-input-' + idx"
               @input="update(idx, $event)"
               :value="values[idx]"
               class="box-input"
               @keyup.delete="moveBack(idx)">
    </div>
</template>

<script>
    export default {
        name: 'vue-chars',

        props: {
            value: {
                required: true
            },

            mask: {
                type: String,
                required: true,
                validator(val) {
                    var truthy;
                    val.split('').forEach(char => {
                        truthy = ['#', 'X', 'S', 'A', 'a'].includes(char);
                    });

                    return truthy;
                }
            }
        },

        data: () => ({
            values: [],
            masks: {
                '#': { pattern: /\d/ },
                'X': { pattern: /[0-9a-zA-Z]/ },
                'S': { pattern: /[a-zA-Z]/ },
                'A': { pattern: /[a-zA-Z]/, transform: v => v.toLocaleUpperCase() },
                'a': { pattern: /[a-zA-Z]/, transform: v => v.toLocaleLowerCase() }
            }
        }),

        created() {
            this.mask.split('').forEach(char => {
                this.values.push(null);
            });

            if (this.value) {
                var arr = this.value.split('');
                this.mask.split('').forEach((char, idx) => {
                    this.values[idx] = arr[idx];
                });
            }
        },

        methods: {
            update(idx, event) {
                var values = event.target.value;
                var ref = this.$refs['box-input-' + idx][0];
                var mask = this.mask.split('')[idx];
                var pattern = this.masks[mask].pattern;
                var oldVal = values.length == 1 || values[0] == undefined ? null : values[0];
                var newVal = values[1] ? values.substr(1, 1) : values.substr(0, 1);

                // Test the mask
                if (!pattern.test(newVal)) {
                    ref.value = oldVal;
                    return;
                }

                ref.value = newVal;
                this.values[idx] = newVal;
                this.$emit('input', this.values.join(''));

                // Focus on the next input
                var next = this.$refs['box-input-' + (idx + 1)];

                if (next) { next[0].focus(); }
            },

            moveBack(idx) {
                var prev = this.$refs['box-input-' + (idx - 1)];

                if (prev) { prev[0].focus(); }
            }
        }
    }
</script>

<style lang="scss">
    $grey: #F2F2F2;

    .boxed-input {
        margin-bottom: 10px;
    }

    .box-input {
        width:          35px;
        border-radius:  5px;
        text-align:     center;
        padding:        8px 10px;
        background:     $grey;
        box-shadow:     none;
        border:         none;
        margin-right:   5px;
        caret-color:    $grey;

        &:focus {
            outline: none;
            box-shadow: 0 0 3px darken($grey, 30%);
        }
    }
</style>
