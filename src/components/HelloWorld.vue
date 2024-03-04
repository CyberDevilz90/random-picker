<script>
export default {
    name: "HelloWorld",
    props: {
        msg: String,
    },
    data() {
        return {
            names: [],
            state: true,
            inputName: "",
            showError: false,
            errorMsg: "",
            result: "",
        };
    },
    computed: {
        isReady() {
            return this.names.length > 1;
        },
    },
    methods: {
        addNameToList() {
            if (this.validate(this.inputName)) {
                this.names.push(this.inputName);
                this.inputName = "";
            } else {
                this.showError = true;
            }
        },
        validate(value) {
            this.errorMsg = "";
            if (value === "") {
                this.errorMsg = "Please enter a valid Name";
                return false;
            }
            if (this.names.includes(value)) {
                this.errorMsg = "Name already taken";
                return false;
            } else {
                return true;
            }
        },
        removeName(i) {
            this.names.splice(i, 1);
        },
        changeState() {
            this.result = this.getRandomNameFromList();
            if (this.result !== "") {
                const index = this.names.indexOf(this.result);
                if (index !== -1) {
                    this.names.splice(index, 1);
                }
            }
            this.state = false;
        },
        getRandomNameFromList() {
            return this.names[Math.floor(Math.random() * this.names.length)]; 
        },
        newRandom() {
            this.names = [];
            this.state = true;
        },
        newResult() {
            if (this.names.length > 1) {
                let availableNames = this.names.filter(
                    (name) => name !== this.result
                ); 
                this.result = this.getRandomNameFromList(availableNames); 
            } else if (this.names.length === 1) {
                this.result = this.names[0]; 
            } else {
                this.result = "";
            }
						this.names.splice(this.names.indexOf(this.result),1)
						if (this.names.length == 0) {
							this.newRandom()
						}
        },
    },
};
</script>

<template>
    <div class="hello" v-if="state">
			<h1 class="title">Random Picker</h1>
        <div class="inputContainer">
            <input class="input" type="text" v-model.trim.lazy="inputName" />
            <button class="addButton" @click="addNameToList">Add</button>
        </div>
        <div class="errorLabel" v-if="showError">
            {{ errorMsg }}
        </div>
				<div class="listNamesContainer">
					<p
            class="listNames"
            v-for="(name, index) in names"
            :key="name"
            @click="removeName(index)"
        >
            {{ name }}
        </p>
				</div>
       
        <div class="checkButton" v-if="isReady" @click="changeState">
            Check the Winner
        </div>
    </div>
    <div class="resultContainer" v-if="!state">
        <h1 class="title">Result</h1>
        <p class="listNames">{{ result }}</p>
        <div class="checkButton" @click="newResult">Random Again</div>
        <div class="checkButton" @click="newRandom">
            Create New Random Picker
        </div>
    </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
@import url("../assets/style/HelloWorld.css");
</style>
