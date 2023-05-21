<template>
    <div class="form-input" :style="{width: width}">
        <input
            class="input-text" 
            :type="type"
            :name="name"
            :id="name"
            :placeholder="placeholder"
            :value="value"
            @input="updateInput"
            />
        <label :for="name" class="input-label">{{ label }}</label>
        <transition-group>
            <div 
                class="form-error"
                v-for="element of error"
                :key="element.$uid">
                <div class="form-error__message">{{ element.$message }}</div>
            </div>
        </transition-group>
    </div>
</template>

<script>
    export default {
        name: 'my-input',
        props: {
            error: {
                type: Array,
                required: false
            },
            value: [String, Number],
            name: {
                type: String,
                required: true
            },
            type: {
                type: String,
                default: 'text'
            },
            placeholder: {
                type: String,
                required: true
            },
            label: {
                type: String,
                required: true
            },
            width: {
                type: String,
                default: '300px'
            },
            
        },
        methods: {
            updateInput(event) {
                this.$emit('update:value', event.target.value);
            }
        }
    }
</script>

<style lang="scss" scoped>
.form {
  &-input {
    margin-bottom: 30px;
    position: relative;
  }
}
.error, .done {
    background: rgb(209, 42, 0);
    box-shadow: 0 0 15px 0 rgb(209, 42, 0, .9);
    margin-top: 4px;
    border-radius: 7px;
    font-size: 13px;
    color: #fff;
    padding: 5px;
}
.done {
    background: rgb(4, 255, 4);
    box-shadow: 0 0 10px 0 rgb(4, 255, 4, .5);
}
.input {
  &-text {
  border: 1px solid rgb(255, 210, 179);
  padding: 0 10px;
  height: 40px;
  border-radius: 7px;
  font-size: 15px;
  width: 268px;
  position: relative;
  z-index: 1;
    &:focus {
        border: 1px solid rgb(209, 87, 0);
      & + .input-label {
        z-index: 1;
        opacity: 1;
        top: -20px;
      }
    }
    &:hover {
        border: 1px solid rgb(209, 87, 0);
    }
    &:not(:placeholder-shown) {
      & + .input-label {
        z-index: 1;
        opacity: 1;
        top: -20px;
      }
    }
  }
  &-label {
    font-weight: bold;
    display: block;
    position: absolute;
    top: 20px;
    opacity: 0;
    z-index: -1;
    transition: .3s;
    font-size: 13px;
    color: var(--primary);
  }
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>