<template>
    <div class="container">
        <editor v-model="code" @init="editorInit" lang="ruby" theme="chrome" width="500" height="100"></editor>
        <button v-on:click="runCode">run</button>

        <p>Status : {{ this.status.status }}</p>
        <p v-if='this.status.compiler_error != ""'>Error : {{ this.status.compiler_error }}</p>
        <p v-if='this.status.compiler_message != ""'>Compiler Messgae : {{ this.status.compiler_message }}</p>
        <p> Program Message : </p>
        <p v-if='this.status.program_message != ""' v-html="this.status.program_message"></p>
        <p v-if='this.status.program_output != ""'> Program OutPut : {{ this.status.program_output }}</p>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data: function() {
        return {
            status: {
                compiler_error: "",
                compiler_message: "",
                program_message: "",
                program_output: "",
                status: 0
            },
            code: ""
        }
    },
    components: {
        editor: require('vue2-ace-editor')
    },
    methods: {
        editorInit: function() {
            require('brace/ext/language_tools')
            require('brace/mode/html')                
            require('brace/mode/ruby')
            require('brace/mode/less')
            require('brace/theme/chrome')
            require('brace/snippets/ruby')
        },
        runCode: function() {
            axios.defaults.headers['Content-type'] = 'application/json'

            axios.post("https://wandbox.org/api/compile.json", { "code":this.code,"options": "", "compiler": "ruby-2.5.1", "compiler-option-raw": "" }).then((response) => {
                console.log(response)
                this.status.compiler_error = response.data.compiler_message
                this.status.compiler_message = response.data.compiler_message
                this.status.program_message = response.data.program_message.replace(/\n/g, "<br>")
                this.status.status = response.data.status 
            }, (error) => {
                console.log(error);
            })
        }
    }
}
</script>