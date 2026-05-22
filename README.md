<!DOCTYPE html>
<html>

<head>

    <meta charset="utf-8">

    <title>Sincronizador</title>

    <style>

        body{
            font-family: Arial;
            padding: 20px;
        }

        textarea{
            width: 400px;
            height: 150px;
        }

        #fraseAtual{
            font-size: 28px;
            margin-top: 30px;
            font-weight: bold;
        }

        #cronometro{
            font-size: 40px;
            margin-top: 20px;
            color: red;
        }

        #resultado{
            margin-top: 30px;
            white-space: pre-line;
        }

    </style>

</head>

<body>

    <h2>Cole os parágrafos</h2>

    <textarea class="colartexto"></textarea>

    <br><br>

    <button onclick="escrever()">
        Confirmar Texto
    </button>

    <hr>

    <h2>Escolha um áudio</h2>

    <input type="file" id="audioFile" accept="audio/*">

    <br><br>

    <button onclick="tocarAudio()">
        Tocar
    </button>

    <button onclick="pausarAudio()">
        Pausar
    </button>

    <button onclick="marcarTempo()">
        Marcar Tempo
    </button>

    <button onclick="reiniciarTudo()">
    Reiniciar
</button>

    <div id="cronometro">
        00:00:000
    </div>

    <div id="fraseAtual">
        Nenhuma frase carregada
    </div>

    <div id="resultado"></div>

    <audio id="player"></audio>

    <script>

        // =========================
        // VARIÁVEIS
        // =========================

        let arrayParagrafos = [];

        let indiceAtual = 0;

        let resultados = [];

        let intervalo;

        let inicio = 0;

        let player =
            document.getElementById("player");

        // =========================
        // PEGAR TEXTO
        // =========================

        function escrever(){

            let confirmaTexto =
                document.getElementsByClassName("colartexto")[0].value;

            arrayParagrafos = confirmaTexto
                .split("\n")
                .filter(item => item.trim() !== "");

            indiceAtual = 0;

            resultados = [];

            if(arrayParagrafos.length > 0){

                document.getElementById("fraseAtual").innerHTML =
                    arrayParagrafos[0];

            }

            console.log(arrayParagrafos);

        }

        // =========================
        // CARREGAR AUDIO
        // =========================

        document
            .getElementById("audioFile")
            .addEventListener("change", function(event){

                let arquivo = event.target.files[0];

                if(arquivo){

                    let urlAudio =
                        URL.createObjectURL(arquivo);

                    player.src = urlAudio;

                }

            });

        // =========================
        // TOCAR AUDIO
        // =========================

        function tocarAudio(){

    // TOCA O AUDIO

    player.play();

    // LIMPA INTERVALOS ANTIGOS

    clearInterval(intervalo);

    // DELAY DE 500ms

    setTimeout(() => {

        // DEFINE O INICIO DO CRONOMETRO

        inicio = Date.now();

        // INICIA O CRONOMETRO

        intervalo = setInterval(() => {

            let tempoAtual =
                Date.now() - inicio;

            // MINUTOS

            let minutos =
                Math.floor(tempoAtual / 60000);

            // SEGUNDOS

            let segundos =
                Math.floor((tempoAtual % 60000) / 1000);

            // MILISEGUNDOS

            let milisegundos =
                Math.floor((tempoAtual % 1000));

            // FORMATAR

            minutos =
                String(minutos).padStart(2, "0");

            segundos =
                String(segundos).padStart(2, "0");

            milisegundos =
                String(milisegundos).padStart(3, "0");

            // MOSTRAR NA TELA

            document.getElementById("cronometro").innerHTML =
                `${minutos}:${segundos}:${milisegundos}`;

        }, 1);

    }, 1100);

}
        // =========================
        // PAUSAR AUDIO
        // =========================

        function pausarAudio(){

            player.pause();

            clearInterval(intervalo);

        }

        // =========================
        // MARCAR TEMPO
        // =========================

        function marcarTempo(){

            if(indiceAtual >= arrayParagrafos.length){

                alert("Todas as frases foram marcadas.");

                return;

            }

            let tempo =
                document.getElementById("cronometro").innerHTML;

            let frase =
                arrayParagrafos[indiceAtual];

            let resultado =
                `[${tempo}] ${frase}`;

            resultados.push(resultado);

            // MOSTRAR RESULTADOS

            document.getElementById("resultado").innerHTML =
                resultados.join("<br>");

            indiceAtual++;

            // MOSTRAR PRÓXIMA FRASE

            if(indiceAtual < arrayParagrafos.length){

                document.getElementById("fraseAtual").innerHTML =
                    arrayParagrafos[indiceAtual];

            }
            else{

                document.getElementById("fraseAtual").innerHTML =
                    "FINALIZADO";

            }

            console.log(resultados);

        }

        function reiniciarTudo(){

    // PARAR AUDIO

    player.pause();

    player.currentTime = 0;

    // PARAR CRONÔMETRO

    clearInterval(intervalo);

    // RESETAR TEMPO

    document.getElementById("cronometro").innerHTML =
        "00:00:000";

    // RESETAR ÍNDICE

    indiceAtual = 0;

    // LIMPAR RESULTADOS

    resultados = [];

    document.getElementById("resultado").innerHTML = "";

    // MOSTRAR PRIMEIRA FRASE NOVAMENTE

    if(arrayParagrafos.length > 0){

        document.getElementById("fraseAtual").innerHTML =
            arrayParagrafos[0];

    }
    else{

        document.getElementById("fraseAtual").innerHTML =
            "Nenhuma frase carregada";

    }

}
    </script>

</body>

</html>
