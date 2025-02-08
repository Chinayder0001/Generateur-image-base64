<!DOCTYPE html>
<html lang="fr-FR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Générateur des images en base64.</title>
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: sans-serif;
        }

        .container {
            width: 100vw;
            height: 100vh;
            background-color: #383839FC;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }

        .imageName {
            width: 90%;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 5px;
            background-color: #111;
            gap: 6px;

        }

        #file {
            width: 100%;
            padding: 10px;
            color: #fff;
        }

        #folder {
            width: 30px;

            padding: 4px;
            background-color: #fff;

        }

        #name {
            width: 100%;
            padding: 3px;
            color: #565A5AFC;
        }

        .baseGenere {
            width: 90%;
            height: 80%;
            background-color: #111;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }

        .base64 {
            width: 100%;
            flex-grow: 1;
            background-color: #222;
            overflow: auto;
            display: flex;
            align-items: center;
        }

        .textarea {
            width: 100%;
            height: 100%;
            background-color: #fff;
            resize: none;
            padding: 5px;
            text-align: justify;
        }

        #testImage {
            width: 100%;
            height: 100%;
            background-color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            display: none;

        }

        #imageView {
            width: 70%;
        }

        .btnCopy {
            width: 100%;
            height: 100px;
            background-color: #222;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 4px;
        }

        #test {
            width: 50%;
        }

        #copy {
            width: 80%;
            height: 50%;
            background-color: #846CDE;
            border: none;
            border-radius: 50px;
            font-size: 100%;
            user-select: none;
        }

        span {
            color: #999;
        }

        #anonce {
            padding: 5px;
            background-color: #fff;
            width: 80px;
            height: 50px;
            position: fixed;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 50px;
            display: none;
        }

        /*----------*/
        #loaded {
            max-width: 70%;
            padding: 15px;
            background-color: #222;
            border-radius: 50px;
            display: flex;
            justify-content: center;
            align-items: center;
            position: fixed;
            bottom: 50%;
            animation: dd 2s ease-in-out infinite;
            display: none;
        }

        @keyframes dd {
            0% {
                opacity: 1;
            }

            100% {
                opacity: 0.2;
            }
        }

        .backConti {
            width: 50px;
            height: 50px;
            background-color: #fff;
            position: fixed;
            top: 6px;
            right: 7px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 50%;
        }

        #continu,
        #back {
            width: 40px;
            height: 40px;
        }

        #continu {
            display: flex;
        }

        #back {
            display: none;
        }
    </style>

</head>

<body>
    <div class="container">
        <div class="imageName">
            <span id="span">Clickez ici pour importer une image</span>
        </div>
        <div class="baseGenere">
            <div class="base64">
                <input style="display: none;" type="file" id="file">
                <textarea placeholder="Pas d'image en base64" id="textarea" readonly class="textarea" rows="8" cols="40"></textarea>

                <div id="testImage">
                    <img id="imageView" scr="">
                </div>
            </div>

            <div class="btnCopy">
                <button id="copy">Copié</button>
            </div>
        </div>

        <div id="loaded">
            <span>Copié</span>
        </div>
        <div id="backContinu" class="backConti">
            <img id="continu" src="data:image/png;base64,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">
            <img id="back" src="data:image/png;base64,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">
        </div>
        
        <span id="anonce">Copié</span>
    </div>
</body>

</html>
<script src="main.js">
    const inputFile = document.getElementById("file");
    const textarea = document.getElementById("textarea");
    const anonce = document.getElementById("anonce");
    const span = document.getElementById("span");
    const copy = document.getElementById("copy");
    const loaded = document.getElementById("loaded")
    const testImage = document.getElementById("testImage");
    const imageView = document.getElementById("imageView");
    const backContinu = document.getElementById("backContinu");
    const continu = document.getElementById("continu");
    const back = document.getElementById("back");

    span.addEventListener("click", () => {
        inputFile.click();
    })

    inputFile.addEventListener("change", (event) => {
        const file = event.target.files[0];
        if (file) {
            const reader = new FileReader();

            reader.onload = function(e) {
                textarea.value = e.target.result;
                imageView.src = e.target.result;

            }
            reader.readAsDataURL(file);
        }

    })
    copy.addEventListener("click", () => {
        if (textarea.value !== "") {
            navigator.clipboard.writeText(textarea.value);
            loaded.style.display = "flex";
        }
        else {
            alert("Importez une image pour la générer en base64");
        }
    })

    setInterval(() => {
        loaded.style.display = 'none';
    }, 10000)
    continu.addEventListener("click", () => {
        continu.style.display = "none";
        back.style.display = "flex";

        textarea.style.display = "none";
        testImage.style.display = "flex"
    })
    back.addEventListener("click", () => {
        continu.style.display = "flex";
        back.style.display = "none";

        textarea.style.display = "inline-block";
        testImage.style.display = "none";
    })
</script>