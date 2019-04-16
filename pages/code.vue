<template>
    <div class="container">
        <editor v-model="code" @init="editorInit" lang="ruby" theme="twilight" width="500" height="100"></editor>
        <button v-on:click="runCode">run</button>

        <p> Output : </p>
        <div class="gray" v-if='this.status.program_message != ""' v-html="this.status.program_message"></div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data: function() {
        return {
            status: {
                program_message: "",
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
            require('brace/theme/twilight')
            require('brace/snippets/ruby')
        },
        runCode: function() {
            axios.defaults.headers['Content-type'] = 'application/json'

            axios.post("https://wandbox.org/api/compile.json", { "code":this.code,"options": "", "compiler": "ruby-2.5.1", "compiler-option-raw": "" }).then((response) => {
                this.status.program_message = response.data.program_message.replace(/\n/g, "<br>")
            }, (error) => {
                console.log(error);
            })
        }
    }
}
</script>

<style>
.gray {
    background-color: dimgray;
    color: azure;
    padding: 10px;
}
</style>
>