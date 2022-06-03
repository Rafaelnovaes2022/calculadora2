<template>
 <div class="calculator">
    <!-- ja vai lê isso diretamente no estado do objeto na função data -->    
    <Display :value="displayValue" />
    <Button label="AC" triple @onClick="clearMemory" class="p-button-sm" />
    <Button label="/" @onClick="setOperation" class="p-button-rounded" />
    <Button label="7" @onClick="addDigit" class="p-button-rounded" />
    <Button label="8" @onClick="addDigit" class="p-button-rounded" />
    <Button label="9" @onClick="addDigit" class="p-button-rounded" />
    <Button label="*" @onClick="setOperation" class="p-button-rounded" />
    <Button label="4" @onClick="addDigit" class="p-button-rounded" />
    <Button label="5" @onClick="addDigit" class="p-button-rounded" />
    <Button label="6" @onClick="addDigit" class="p-button-rounded" />
    <Button label="-" @onClick="setOperation" class="p-button-rounded" />
    <Button label="1" @onClick="addDigit" class="p-button-rounded" />
    <Button label="2" @onClick="addDigit" class="p-button-rounded" />
    <Button label="3" @onClick="addDigit" class="p-button-rounded" />
    <Button label="+" @onClick="setOperation" class="p-button-rounded" />
    <Button label="0" double @onClick="addDigit" class="p-button-rounded" />
    <Button label="." @onClick="addDigit" class="p-button-rounded" />
    <Button label="=" @onClick="setOperation" class="p-button-rounded" />
    
    </div>  
    
</template>

<script>
import Button from "../components/Button"
import Display from "../components/Display"
export default {
    data: function() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0
        }
    },
    components: { Button, Display },
    methods: {
        clearMemory() {
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {

            if (this.current === 0) {
                this.operation = operation;
                this.current = 1;
                this.clearDisplay = true;

               } else {
                const equals = operation === "=";
                const currentOperation = this.operation;
                    
                    try {
                    this.values[0] = eval(
                    `${this.values[0]} ${currentOperation} ${this.values[1]}`
                     );
                    } catch (e) {
                        this.$emit('onError', e)

                    }

                    this.values[1] = 0

                    this.displayValue = this.values[0]
                    this.operation = equals ? null : operation
                    this.current = equals ? 0 : 1
                    this.clearDisplay = !equals
            }
        },
        addDigit(n) {
            if (n === "." && this.displayValue.includes(".")) {
                return            
        }
            const clearDisplay = this.displayValue === "0"
            || this.clearDisplay
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n
            
            this.displayValue = displayValue
            this.clearDisplay = false

            if (n !== ".") {
                const i = this.current
                const newValue = parseFloat(displayValue)
                this.values[i] = newValue
            }
    }
}


}

</script>

<style>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
    background-color: black;
    
}
</style>