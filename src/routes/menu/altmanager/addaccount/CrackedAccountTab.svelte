<script lang="ts">
    import Tab from "../../common/modal/Tab.svelte";
    import IconTextInput from "../../common/setting/IconTextInput.svelte";
    import ButtonSetting from "../../common/setting/ButtonSetting.svelte";
    import {addCrackedAccount, randomCatName, randomInternetUsername, randomSillyName, randomUsername} from "../../../../integration/rest";
    import IconButton from "../../common/buttons/IconButton.svelte";
    import SwitchSetting from "../../common/setting/SwitchSetting.svelte";
    import SingleSelect from "../../common/setting/select/SingleSelect.svelte";

    let username = "";
    let online = false;
    let mode = "Normal"

    async function addAccount() {
        await addCrackedAccount(username, online);
    }

    async function generateRandomUsername() {
        switch(mode) {
            case "Internet":
                username = await randomInternetUsername();
                return;
            case "Cat":
                username = await randomCatName();
                return;
            case "Silly":
                username = await randomSillyName();
                return;
        }

        username = await randomUsername();

    }
</script>

<Tab>
    <IconTextInput icon="user" title="Username" bind:value={username} maxLength={16}>
        <IconButton icon="random" title="Random" on:click={generateRandomUsername}/>
    </IconTextInput>
    <SwitchSetting title="Use online UUID" bind:value={online}/>
    <SingleSelect 
        title="Generator"
        options={["Normal", "Internet", "Cat", "Silly"]}
        bind:value={mode}
    />
    <ButtonSetting title="Add Account" on:click={addAccount} listenForEnter={true} inset={true}/>
</Tab>
