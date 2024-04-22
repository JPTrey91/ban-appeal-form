<script>
    const FORM_ACCESS_KEY = import.meta.env.VITE_FORM_ACCESS_KEY;

    let form;
    let result;

    function onSubmit(e) {
        e.preventDefault();
        const formData = new FormData(form);
        const object = Object.fromEntries(formData);
        const json = JSON.stringify(object);
        result.innerHTML = "Please wait...";

        fetch("https://api.web3forms.com/submit", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Accept: "application/json",
            },
            body: json,
        })
            .then(async (response) => {
                let json = await response.json();
                if (response.status == 200) {
                    result.innerHTML = json.message;
                } else {
                    console.log(response);
                    result.innerHTML = json.message;
                }
            })
            .catch((error) => {
                console.log(error);
                result.innerHTML = "Something went wrong!";
            })
            .then(function () {
                form.reset();
                setTimeout(() => {
                    result.style.display = "none";
                }, 3000);
            });
    }
</script>

<main>
    <h1>FenAster Appeal Form</h1>

    <form method="POST" id="form" bind:this={form} on:submit={onSubmit}>
        <input type="hidden" name="access_key" value={FORM_ACCESS_KEY} />
        <input type="hidden" name="subject" value="New Ban Appeal" />
        <input type="checkbox" name="botcheck" id="" style="display: none;" />

        <!-- Custom Form Data -->
        <input
            type="text"
            name="username"
            placeholder="Discord Username"
            required
        />
        <input
            type="text"
            name="reason"
            placeholder="Official Reason You Were Banned"
            required
        />
        <textarea name="message" placeholder="Your appeal..." required
        ></textarea>

        <button type="submit">Submit</button>

        <div id="result" bind:this={result}></div>
    </form>
</main>

<style>
    form {
        display: flex;
        flex-direction: column;
        gap: 1rem;
        width: 750px;
    }

    input,
    textarea {
        border-radius: 4px;
        padding: 0.6rem 0.4rem;
        width: 100%;
        font-size: 1.2em;
    }

    input {
        height: 40px;
    }

    textarea {
        height: 180px;
    }

    button {
        background-color: lightblue;
        border: none;
        width: 100%;

        &:hover {
            background-color: turquoise;
        }
    }
</style>
