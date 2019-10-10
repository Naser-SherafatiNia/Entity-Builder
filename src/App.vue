<template>
    <div id="app">
        <Bar></Bar>

        <SideBar></SideBar>

        <div class="container">
            <Builder class="content"></Builder>
        </div>

        <CodeDialogue></CodeDialogue>
        <ListDialogue></ListDialogue>
    </div>
</template>

<script>
    import Bar from './components/Bar';
    import SideBar from './components/SideBar';
    import Builder from './components/Builder';
    import CodeDialogue from './components/CodeDialogue';
    import ListDialogue from './components/ListDialogue';

    export default {
        name: 'App',
        components: {
            Bar,
            SideBar,
            Builder,
            CodeDialogue,
            ListDialogue
        },
        data() {
            return {};
        },
        created() {},
        methods: {}
    };
</script>

<style>
    .content {
        padding-left: 120px;
    }

    .file-title {
        line-height: 3;
        display: inline-block;
    }

    .title h1 {
        display: inline;
        margin-right: 4px;
    }

    .title .btn-group {
        margin-right: 4px
    }

    .button-cell:hover .btn-group {
        display: inline-block;
    }

    .button-cell .btn-group {
        display: none;
    }

    .checkbox {
        zoom: 2;
        display: inline;
    }

    .btn-primary.plus {
        width: 100%;
        font-size: large;
        font-weight: bold;
    }
</style>
