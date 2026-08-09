<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Laboratório TCP/IP</title>

<style>

*{
    box-sizing:border-box;
}

:root{
    --bg:#07111f;
    --panel:#0d1b2e;
    --panel2:#10233b;
    --border:#23415f;
    --text:#eaf4ff;
    --muted:#9bb0c7;
    --blue:#2494ff;
    --orange:#ff9f32;
    --green:#27d17f;
    --red:#ff5364;
}

body{
    margin:0;
    min-height:100vh;
    background:
        radial-gradient(circle at 20% 0%,#12345a 0,#07111f 40%),
        #07111f;
    color:var(--text);
    font-family:Arial,Helvetica,sans-serif;
}

button{
    font-family:inherit;
    cursor:pointer;
    color:white;
    background:#142941;
    border:1px solid #31516e;
    border-radius:8px;
    padding:9px 13px;
    font-weight:bold;
    transition:.18s;
}

button:hover{
    background:#1b3959;
    transform:translateY(-1px);
}

button.primary{
    background:#126dcc;
    border-color:#288eea;
}

button.danger{
    background:#8e2635;
    border-color:#d44356;
}

.header{
    padding:18px 24px;
    border-bottom:1px solid var(--border);
    background:#071321;
    display:flex;
    justify-content:space-between;
    align-items:center;
    gap:20px;
    position:sticky;
    top:0;
    z-index:100;
}

.logo h1{
    margin:0;
    font-size:24px;
}

.logo span{
    color:var(--muted);
    font-size:13px;
}

.header-controls{
    display:flex;
    flex-wrap:wrap;
    gap:7px;
    justify-content:flex-end;
}

.protocol-selector{
    display:flex;
    gap:4px;
    background:#081522;
    padding:4px;
    border-radius:9px;
}

.protocol-selector button{
    min-width:80px;
}

.tcp.active{
    background:#126dcc;
}

.udp.active{
    background:#c56d0e;
}

.layout{
    max-width:1500px;
    margin:auto;
    padding:18px;
    display:grid;
    grid-template-columns:250px minmax(500px,1fr) 290px;
    gap:16px;
}

.panel{
    background:linear-gradient(180deg,#0e2035,#0b192a);
    border:1px solid var(--border);
    border-radius:12px;
    box-shadow:0 10px 30px rgba(0,0,0,.28);
    overflow:hidden;
}

.panel-title{
    padding:12px 14px;
    font-weight:bold;
    font-size:13px;
    letter-spacing:.5px;
    color:#cfe6ff;
    background:#0a1727;
    border-bottom:1px solid var(--border);
}

.control{
    padding:12px;
}

.control-group{
    padding-bottom:14px;
    margin-bottom:14px;
    border-bottom:1px solid #1b334c;
}

.control-group:last-child{
    border-bottom:0;
}

.control-title{
    color:#8fb0ce;
    font-size:12px;
    text-transform:uppercase;
    margin-bottom:8px;
    font-weight:bold;
}

.big-button{
    width:100%;
    margin-bottom:7px;
    padding:11px;
}

.info-box{
    background:#091624;
    border:1px solid #1c3854;
    padding:10px;
    border-radius:8px;
    color:#b8cde1;
    font-size:12px;
    line-height:1.5;
}

.application{
    margin-bottom:12px;
}

.application-content{
    padding:13px 16px;
    display:flex;
    gap:14px;
    align-items:center;
}

.app-icon{
    width:46px;
    height:46px;
    display:grid;
    place-items:center;
    background:#152b43;
    border-radius:10px;
    font-size:25px;
}

.label{
    color:#7f9ab6;
    font-size:11px;
    text-transform:uppercase;
    margin-bottom:4px;
}

.message{
    font-size:18px;
    font-weight:bold;
}

.network-panel{
    margin-bottom:12px;
}

.network{
    height:475px;
    position:relative;
    overflow:hidden;
    background:
        radial-gradient(circle at 50% 50%,rgba(26,87,133,.13),transparent 55%),
        #081523;
}

.link{
    position:absolute;
    height:4px;
    background:#29445d;
    border-radius:5px;
    transition:.4s;
}

.link.active{
    background:#2494ff;
    box-shadow:0 0 12px #2494ff;
}

.l1{
    left:11%;
    top:50%;
    width:18%;
}

.l2{
    left:31%;
    top:50%;
    width:13%;
}

.l3{
    left:44%;
    top:50%;
    width:15%;
}

.l4{
    left:59%;
    top:50%;
    width:15%;
}

.l5{
    left:74%;
    top:50%;
    width:13%;
}

.l6{
    left:87%;
    top:50%;
    width:8%;
}

.node{
    position:absolute;
    transform:translate(-50%,-50%);
    text-align:center;
    width:110px;
    z-index:5;
}

.node-a{
    left:8%;
    top:50%;
}

.node-sw1{
    left:30%;
    top:50%;
}

.node-r1{
    left:43%;
    top:50%;
}

.node-r2{
    left:60%;
    top:50%;
}

.node-sw2{
    left:75%;
    top:50%;
}

.node-b{
    left:93%;
    top:50%;
}

.node-icon{
    width:58px;
    height:58px;
    margin:auto;
    border-radius:13px;
    background:#122b44;
    border:1px solid #315473;
    display:grid;
    place-items:center;
    font-size:28px;
}

.node-name{
    margin-top:7px;
    font-size:12px;
    font-weight:bold;
}

.node-ip{
    color:#7594b1;
    font-size:9px;
    margin-top:3px;
}

.internet{
    position:absolute;
    left:51.5%;
    top:22%;
    transform:translate(-50%,-50%);
    color:#7ca8cb;
    text-align:center;
    font-size:11px;
    line-height:1.5;
}

.packet{
    position:absolute;
    width:70px;
    min-height:31px;
    padding:7px 8px;
    background:#126dcc;
    border:2px solid #72c4ff;
    border-radius:7px;
    color:white;
    font-size:10px;
    font-weight:bold;
    text-align:center;
    z-index:20;
    box-shadow:0 0 15px rgba(36,148,255,.55);
    cursor:pointer;
}

.packet.udp{
    background:#c66e0e;
    border-color:#ffc36f;
}

.packet.ack{
    background:#168252;
    border-color:#73e5ad;
}

.packet.lost{
    background:#92283a;
    border-color:#ff7180;
}

.event-panel{
    margin-bottom:12px;
}

.event{
    min-height:110px;
    padding:15px;
    display:flex;
    align-items:center;
    gap:15px;
}

.event-icon{
    width:55px;
    height:55px;
    min-width:55px;
    border-radius:50%;
    background:#132a43;
    display:grid;
    place-items:center;
    font-size:27px;
    border:1px solid #31516e;
}

.event-icon.tcp{
    border-color:#288eea;
}

.event-icon.udp{
    border-color:var(--orange);
}

.event-icon.loss{
    border-color:var(--red);
}

.event-title{
    font-size:17px;
    font-weight:bold;
    margin-bottom:5px;
}

.event-description{
    color:#a9bfd4;
    font-size:13px;
    line-height:1.45;
}

.auto-progress{
    height:5px;
    background:#15293d;
    border-radius:10px;
    overflow:hidden;
    margin-top:10px;
}

.auto-progress-bar{
    height:100%;
    width:0;
    background:var(--blue);
    transition:width .2s;
}

.timeline{
    padding:17px 18px 20px;
}

.timeline-track{
    position:relative;
    height:70px;
}

.timeline-line{
    position:absolute;
    left:5%;
    right:5%;
    top:16px;
    height:3px;
    background:#29435c;
}

.timeline-events{
    display:flex;
    justify-content:space-between;
    position:relative;
}

.timeline-item{
    text-align:center;
    width:16%;
}

.timeline-dot{
    width:18px;
    height:18px;
    margin:7px auto 10px;
    border-radius:50%;
    background:#263d54;
    border:3px solid #52708b;
}

.timeline-item.active .timeline-dot{
    background:var(--blue);
    border-color:#a8ddff;
    box-shadow:0 0 12px var(--blue);
}

.timeline-item.done .timeline-dot{
    background:var(--green);
    border-color:#9bf0c8;
}

.timeline-label{
    font-size:10px;
    color:#8fa8c0;
}

.stack{
    padding:12px;
}

.layer{
    padding:11px;
    margin-bottom:7px;
    border-radius:7px;
    border:1px solid #294762;
    background:#10253b;
    display:flex;
    justify-content:space-between;
    align-items:center;
    transition:.3s;
}

.layer.active{
    background:#143b5d;
    border-color:#3099ee;
    box-shadow:0 0 12px rgba(36,148,255,.18);
}

.layer-name{
    font-size:12px;
    font-weight:bold;
}

.layer-info{
    font-size:10px;
    color:#8faecc;
}

.inspector{
    padding:12px;
}

.empty{
    color:#7189a0;
    font-size:12px;
    line-height:1.5;
    padding:12px;
    text-align:center;
}

.packet-card{
    border:1px solid #284762;
    background:#091725;
    border-radius:8px;
    overflow:hidden;
}

.packet-card-header{
    padding:10px;
    background:#112b44;
    display:flex;
    justify-content:space-between;
    font-size:12px;
    font-weight:bold;
}

.field{
    display:flex;
    justify-content:space-between;
    gap:10px;
    padding:7px 9px;
    border-top:1px solid #172f46;
    font-size:11px;
}

.field span:first-child{
    color:#7794ae;
}

.field span:last-child{
    color:#e1effd;
    text-align:right;
}

.message-log{
    max-height:230px;
    overflow-y:auto;
    padding:10px;
}

.log{
    padding:9px;
    margin-bottom:7px;
    border-left:3px solid var(--blue);
    background:#091725;
    border-radius:4px;
    font-size:11px;
    color:#b8cee2;
    line-height:1.4;
}

.log.tcp{
    border-color:var(--blue);
}

.log.udp{
    border-color:var(--orange);
}

.log.loss{
    border-color:var(--red);
}

.log.success{
    border-color:var(--green);
}

.footer-note{
    color:#7189a0;
    font-size:10px;
    text-align:center;
    padding:10px;
}

@media(max-width:1100px){

    .layout{
        grid-template-columns:220px 1fr;
    }

    .right-column{
        grid-column:1/-1;
        display:grid;
        grid-template-columns:1fr 1fr;
        gap:12px;
    }

}

@media(max-width:800px){

    .header{
        position:static;
        flex-direction:column;
        align-items:flex-start;
    }

    .layout{
        grid-template-columns:1fr;
    }

    .right-column{
        display:block;
    }

    .network{
        height:420px;
    }

}

</style>
</head>


<body>


<header class="header">

    <div class="logo">

        <h1>
            🧪 Laboratório TCP/IP
        </h1>

        <span>
            Observe os protocolos funcionando
        </span>

    </div>


    <div class="header-controls">

        <button
            class="primary"
            onclick="sendMessage()">

            📨 Enviar mensagem

        </button>


        <button
            onclick="startTCP()">

            🤝 Conexão TCP

        </button>


        <button
            class="danger"
            onclick="armLoss()">

            💥 Simular perda

        </button>


        <button
            onclick="stepSimulation()">

            ⏭ Próxima etapa

        </button>


        <div class="protocol-selector">

            <button
                id="tcpButton"
                class="tcp active"
                onclick="setProtocol('TCP')">

                🔵 TCP

            </button>


            <button
                id="udpButton"
                class="udp"
                onclick="setProtocol('UDP')">

                🟠 UDP

            </button>

        </div>

    </div>

</header>



<main class="layout">


<!-- =====================================================
     CONTROLE
===================================================== -->

<aside class="panel">

    <div class="panel-title">
        🎛 CONTROLE
    </div>


    <div class="control">


        <div class="control-group">

            <div class="control-title">
                Comunicação
            </div>


            <button
                class="big-button primary"
                onclick="sendMessage()">

                📨 Enviar dados

            </button>


            <button
                class="big-button"
                onclick="startTCP()">

                🤝 Fazer handshake TCP

            </button>


            <button
                class="big-button"
                onclick="sendACKManually()">

                ✔ Enviar ACK

            </button>

        </div>



        <div class="control-group">

            <div class="control-title">
                Experimentos
            </div>


            <button
                class="big-button danger"
                onclick="armLoss()">

                💥 Perder próximo pacote

            </button>


            <button
                class="big-button"
                onclick="duplicatePacket()">

                📋 Duplicar pacote

            </button>

        </div>



        <div class="control-group">

            <div class="control-title">
                Modo de aula
            </div>


            <button
                class="big-button"
                id="autoButton"
                onclick="toggleAuto()">

                ▶ Modo automático

            </button>


            <button
                class="big-button"
                onclick="resetSimulation()">

                🔄 Reiniciar laboratório

            </button>

        </div>



        <div class="control-group">

            <div class="control-title">
                Situação atual
            </div>


            <div
                class="info-box"
                id="controlInfo">

                TCP selecionado.

                <br><br>

                Laboratório pronto.

            </div>

        </div>



        <div class="control-group">

            <div class="control-title">
                💡 Dica para o professor
            </div>


            <div class="info-box">

                Primeiro faça os alunos
                observarem.

                <br><br>

                Depois pergunte:

                <br><br>

                <b>
                "O que vocês acham que acabou de acontecer?"
                </b>

            </div>

        </div>

    </div>

</aside>



<!-- =====================================================
     ÁREA CENTRAL
===================================================== -->

<section class="center">


    <div class="panel application">

        <div class="panel-title">
            📝 APLICAÇÃO
        </div>


        <div class="application-content">

            <div class="app-icon">
                📝
            </div>


            <div>

                <div class="label">
                    Mensagem gerada pela aplicação
                </div>


                <div
                    class="message"
                    id="applicationMessage">

                    "Olá, servidor!"

                </div>

            </div>

        </div>

    </div>



    <!-- REDE -->

    <div class="panel network-panel">

        <div class="panel-title">

            🌐 REDE — visualização da comunicação

        </div>


        <div
            class="network"
            id="network">


            <div
                class="link l1"
                id="link1">
            </div>


            <div
                class="link l2"
                id="link2">
            </div>


            <div
                class="link l3"
                id="link3">
            </div>


            <div
                class="link l4"
                id="link4">
            </div>


            <div
                class="link l5"
                id="link5">
            </div>


            <div
                class="link l6"
                id="link6">
            </div>



            <div class="node node-a">

                <div class="node-icon">
                    💻
                </div>

                <div class="node-name">
                    Computador A
                </div>

                <div class="node-ip">
                    192.168.1.10
                </div>

            </div>



            <div class="node node-sw1">

                <div class="node-icon">
                    🔀
                </div>

                <div class="node-name">
                    Switch
                </div>

                <div class="node-ip">
                    rede local
                </div>

            </div>



            <div class="node node-r1">

                <div class="node-icon">
                    📡
                </div>

                <div class="node-name">
                    Roteador
                </div>

                <div class="node-ip">
                    gateway
                </div>

            </div>



            <div class="internet">

                ☁️

                <br>

                INTERNET

            </div>



            <div class="node node-r2">

                <div class="node-icon">
                    📡
                </div>

                <div class="node-name">
                    Roteador
                </div>

                <div class="node-ip">
                    rede destino
                </div>

            </div>



            <div class="node node-sw2">

                <div class="node-icon">
                    🔀
                </div>

                <div class="node-name">
                    Switch
                </div>

                <div class="node-ip">
                    rede local
                </div>

            </div>



            <div class="node node-b">

                <div class="node-icon">
                    🖥️
                </div>

                <div class="node-name">
                    Servidor B
                </div>

                <div class="node-ip">
                    192.168.2.20
                </div>

            </div>


        </div>

    </div>



    <!-- ACOMPANHAMENTO -->

    <div class="panel event-panel">

        <div class="panel-title">

            📢 ACOMPANHAMENTO DA AÇÃO

        </div>


        <div class="event">


            <div
                class="event-icon"
                id="eventIcon">

                📡

            </div>


            <div style="width:100%">

                <div
                    class="event-title"
                    id="eventTitle">

                    Laboratório pronto

                </div>


                <div
                    class="event-description"
                    id="eventDescription">

                    Escolha um protocolo e envie uma mensagem.

                </div>


                <div class="auto-progress">

                    <div
                        class="auto-progress-bar"
                        id="progressBar">

                    </div>

                </div>

            </div>

        </div>

    </div>



    <!-- LINHA DO TEMPO -->

    <div class="panel">

        <div class="panel-title">

            🧭 LINHA DO TEMPO DA COMUNICAÇÃO

        </div>


        <div class="timeline">

            <div class="timeline-track">

                <div class="timeline-line"></div>


                <div class="timeline-events">


                    <div
                        class="timeline-item"
                        id="t1">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            Aplicação
                        </div>

                    </div>


                    <div
                        class="timeline-item"
                        id="t2">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            Transporte
                        </div>

                    </div>


                    <div
                        class="timeline-item"
                        id="t3">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            IP
                        </div>

                    </div>


                    <div
                        class="timeline-item"
                        id="t4">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            Ethernet
                        </div>

                    </div>


                    <div
                        class="timeline-item"
                        id="t5">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            Rede
                        </div>

                    </div>


                    <div
                        class="timeline-item"
                        id="t6">

                        <div class="timeline-dot"></div>

                        <div class="timeline-label">
                            Servidor
                        </div>

                    </div>


                </div>

            </div>

        </div>

    </div>

</section>



<!-- =====================================================
     LATERAL DIREITA
===================================================== -->

<aside class="right-column">


    <!-- PILHA -->

    <div class="panel">

        <div class="panel-title">

            🧱 PILHA DE PROTOCOLOS

        </div>


        <div class="stack">


            <div
                class="layer"
                id="layer-app">

                <span class="layer-name">
                    7 — Aplicação
                </span>

                <span class="layer-info">
                    HTTP / dados
                </span>

            </div>


            <div
                class="layer"
                id="layer-transport">

                <span class="layer-name">
                    4 — Transporte
                </span>

                <span
                    class="layer-info"
                    id="transportLabel">

                    TCP

                </span>

            </div>


            <div
                class="layer"
                id="layer-ip">

                <span class="layer-name">
                    3 — Internet
                </span>

                <span class="layer-info">
                    IPv4
                </span>

            </div>


            <div
                class="layer"
                id="layer-ethernet">

                <span class="layer-name">
                    2 — Enlace
                </span>

                <span class="layer-info">
                    Ethernet
                </span>

            </div>


            <div class="info-box">

                <b>
                    Encapsulamento
                </b>

                <br><br>

                A mensagem desce pela pilha.

                <br><br>

                Cada camada acrescenta
                informações necessárias
                para o transporte.

            </div>

        </div>

    </div>



    <!-- INSPEÇÃO -->

    <div class="panel">

        <div class="panel-title">

            🔎 INSPEÇÃO DO PACOTE

        </div>


        <div
            class="inspector"
            id="inspector">

            <div class="empty">

                Clique em um pacote durante
                a simulação para examinar
                seus cabeçalhos.

            </div>

        </div>

    </div>



    <!-- EVENTOS -->

    <div class="panel">

        <div class="panel-title">

            📜 EVENTOS

        </div>


        <div
            class="message-log"
            id="messageLog">

            <div class="log">

                Laboratório iniciado.

            </div>

        </div>

    </div>


</aside>

</main>



<div class="footer-note">

    Laboratório didático — Introdução aos protocolos TCP/IP e UDP

</div>



<script>


/* =========================================================
   VARIÁVEIS
========================================================= */

let protocol = "TCP";

let lossArmed = false;

let tcpConnected = false;

let sequence = 1000;

let currentPacket = null;

let autoRunning = false;


/* =========================================================
   ELEMENTOS
========================================================= */

const network =
    document.getElementById("network");

const eventTitle =
    document.getElementById("eventTitle");

const eventDescription =
    document.getElementById("eventDescription");

const eventIcon =
    document.getElementById("eventIcon");

const inspector =
    document.getElementById("inspector");

const controlInfo =
    document.getElementById("controlInfo");

const transportLabel =
    document.getElementById("transportLabel");

const progressBar =
    document.getElementById("progressBar");

const messageLog =
    document.getElementById("messageLog");


/* =========================================================
   ESPERA
========================================================= */

function sleep(ms){

    return new Promise(
        resolve => setTimeout(resolve,ms)
    );

}


/* =========================================================
   LOG
========================================================= */

function logEvent(text,type=""){

    const div =
        document.createElement("div");

    div.className =
        "log " + type;

    div.innerHTML = text;

    messageLog.prepend(div);

}


/* =========================================================
   MENSAGEM PRINCIPAL
========================================================= */

function setEvent(
    title,
    description,
    icon="📡",
    type=""
){

    eventTitle.textContent =
        title;

    eventDescription.textContent =
        description;

    eventIcon.textContent =
        icon;

    eventIcon.className =
        "event-icon " + type;

}


/* =========================================================
   TIMELINE
========================================================= */

function clearTimeline(){

    for(let i=1;i<=6;i++){

        document
            .getElementById("t"+i)
            .classList
            .remove("active","done");

    }

}


function activateTimeline(n){

    for(let i=1;i<n;i++){

        const item =
            document.getElementById("t"+i);

        item.classList.remove("active");

        item.classList.add("done");

    }

    document
        .getElementById("t"+n)
        .classList.add("active");

}


function finishTimeline(){

    for(let i=1;i<=6;i++){

        const item =
            document.getElementById("t"+i);

        item.classList.remove("active");

        item.classList.add("done");

    }

}


/* =========================================================
   CAMADA ATIVA
========================================================= */

function highlightLayer(id){

    document
        .querySelectorAll(".layer")
        .forEach(layer=>{
            layer.classList.remove("active");
        });

    document
        .getElementById(id)
        .classList.add("active");

}


/* =========================================================
   LINKS
========================================================= */

function clearLinks(){

    document
        .querySelectorAll(".link")
        .forEach(link=>{
            link.classList.remove("active");
        });

}


/* =========================================================
   PROTOCOLO
========================================================= */

function setProtocol(p){

    protocol = p;

    tcpConnected = false;

    document
        .getElementById("tcpButton")
        .classList
        .toggle("active",p==="TCP");

    document
        .getElementById("udpButton")
        .classList
        .toggle("active",p==="UDP");

    transportLabel.textContent = p;

    clearTimeline();

    clearLinks();


    if(p==="TCP"){

        setEvent(
            "TCP selecionado",
            "O TCP trabalha com conexão, confirmação e retransmissão.",
            "🔵",
            "tcp"
        );

        controlInfo.innerHTML =
            "<b>TCP selecionado.</b><br><br>" +
            "Faça o handshake antes de enviar os dados.";

    }

    else{

        setEvent(
            "UDP selecionado",
            "O UDP envia os dados sem estabelecer uma conexão TCP.",
            "🟠",
            "udp"
        );

        controlInfo.innerHTML =
            "<b>UDP selecionado.</b><br><br>" +
            "Não possui confirmação nem retransmissão automática.";

    }

}


/* =========================================================
   INSPEÇÃO
========================================================= */

function inspectPacket(data){

    currentPacket = data;

    inspector.innerHTML = `

        <div class="packet-card">

            <div
                class="packet-card-header"
                style="border-left:4px solid ${
                    data.protocol==="TCP"
                    ? "#2494ff"
                    : "#ff9f32"
                }">

                <span>
                    ${data.type}
                </span>

                <span>
                    ${data.protocol}
                </span>

            </div>


            <div class="field">
                <span>Origem</span>
                <span>${data.source}</span>
            </div>


            <div class="field">
                <span>Destino</span>
                <span>${data.destination}</span>
            </div>


            <div class="field">
                <span>IP origem</span>
                <span>192.168.1.10</span>
            </div>


            <div class="field">
                <span>IP destino</span>
                <span>192.168.2.20</span>
            </div>


            <div class="field">
                <span>Porta origem</span>
                <span>${data.sourcePort}</span>
            </div>


            <div class="field">
                <span>Porta destino</span>
                <span>${data.destinationPort}</span>
            </div>


            <div class="field">
                <span>Número de sequência</span>
                <span>${data.sequence}</span>
            </div>


            <div class="field">
                <span>ACK</span>
                <span>${data.ack}</span>
            </div>


            <div class="field">
                <span>Dados</span>
                <span>${data.payload || "—"}</span>
            </div>

        </div>

    `;

}


/* =========================================================
   CRIA PACOTE
========================================================= */

function createPacket(data){

    const packet =
        document.createElement("div");

    packet.className =
        "packet " +
        (data.protocol==="UDP" ? "udp " : "") +
        (data.type==="ACK" ? "ack" : "");

    packet.textContent =
        data.type==="ACK"
        ? "ACK"
        : data.protocol;


    packet.style.left = "7%";

    packet.style.top = "42%";


    packet.onclick = function(event){

        event.stopPropagation();

        inspectPacket(data);

    };


    network.appendChild(packet);

    return packet;

}


/* =========================================================
   CAMINHO
========================================================= */

const forwardPositions = [

    {
        left:"7%",
        top:"42%"
    },

    {
        left:"27%",
        top:"42%"
    },

    {
        left:"40%",
        top:"42%"
    },

    {
        left:"57%",
        top:"42%"
    },

    {
        left:"72%",
        top:"42%"
    },

    {
        left:"90%",
        top:"42%"
    }

];


/* =========================================================
   MOVIMENTO
========================================================= */

async function movePacket(
    packet,
    positions,
    data
){

    for(let i=0;i<positions.length;i++){

        packet.style.transition =
            "left 1.1s ease, top .7s ease";

        packet.style.left =
            positions[i].left;

        packet.style.top =
            positions[i].top;


        progressBar.style.width =
            ((i+1)/positions.length)*100 + "%";


        if(i < positions.length-1){

            await sleep(1100);

        }

    }


    inspectPacket(data);

}


/* =========================================================
   HANDSHAKE TCP
========================================================= */

async function startTCP(){

    if(protocol!=="TCP"){

        setEvent(
            "Handshake é um mecanismo do TCP",
            "Selecione TCP para visualizar o estabelecimento da conexão.",
            "⚠️"
        );

        return;

    }


    if(tcpConnected){

        setEvent(
            "Conexão TCP já estabelecida",
            "O computador A já possui uma conexão com o servidor B.",
            "🔗",
            "tcp"
        );

        return;

    }


    clearTimeline();

    clearLinks();


    controlInfo.innerHTML =
        "<b>Estabelecendo conexão TCP...</b><br><br>" +
        "Observe as três etapas do handshake.";


    /* SYN */

    activateTimeline(1);

    highlightLayer("layer-transport");


    setEvent(
        "1 — SYN",
        "O computador A solicita ao servidor o estabelecimento de uma conexão TCP.",
        "📤",
        "tcp"
    );


    logEvent(
        "<b>SYN</b> — solicitação de conexão TCP.",
        "tcp"
    );


    await sleep(5000);


    /* SYN ACK */

    activateTimeline(2);

    highlightLayer("layer-transport");


    setEvent(
        "2 — SYN/ACK",
        "O servidor recebeu o SYN e respondeu aceitando a conexão.",
        "📥",
        "tcp"
    );


    logEvent(
        "<b>SYN/ACK</b> — servidor respondeu.",
        "tcp"
    );


    await sleep(5000);


    /* ACK */

    activateTimeline(3);


    setEvent(
        "3 — ACK",
        "O computador A confirma a resposta. A conexão TCP está estabelecida.",
        "✔",
        "tcp"
    );


    logEvent(
        "<b>ACK</b> — conexão TCP estabelecida.",
        "success"
    );


    tcpConnected = true;


    await sleep(5000);


    finishTimeline();


    controlInfo.innerHTML =
        "<b>TCP conectado.</b><br><br>" +
        "Agora os dados podem ser enviados.";

}


/* =========================================================
   ENVIO
========================================================= */

async function sendMessage(){

    if(autoRunning===false){

    }


    if(protocol==="TCP" && !tcpConnected){

        setEvent(
            "TCP ainda não está conectado",
            "Faça primeiro o handshake TCP.",
            "⚠️"
        );

        controlInfo.innerHTML =
            "<b>Atenção:</b> faça primeiro o handshake.";

        return;

    }


    clearTimeline();

    clearLinks();

    progressBar.style.width="0%";


    const payload =
        document
        .getElementById("applicationMessage")
        .textContent
        .replaceAll('"','');


    const data = {

        type:"DADOS",

        protocol:protocol,

        source:"Computador A",

        destination:"Servidor B",

        sourcePort:49152,

        destinationPort:80,

        sequence:sequence,

        ack:
            protocol==="TCP"
            ? "esperado"
            : "não utilizado",

        payload:payload

    };


    /* APLICAÇÃO */

    activateTimeline(1);

    highlightLayer("layer-app");


    setEvent(
        "Aplicação criou os dados",
        "A aplicação produziu a mensagem que precisa ser enviada.",
        "📝",
        protocol==="TCP" ? "tcp" : "udp"
    );


    logEvent(
        "Aplicação criou: <b>\"" +
        payload +
        "\"</b>",
        protocol==="TCP" ? "tcp" : "udp"
    );


    await sleep(5000);


    /* TRANSPORTE */

    activateTimeline(2);

    highlightLayer("layer-transport");


    if(protocol==="TCP"){

        setEvent(
            "TCP adicionou informações de controle",
            "O TCP adiciona portas, sequência e mecanismos para controlar a entrega.",
            "🔵",
            "tcp"
        );

        logEvent(
            "<b>TCP:</b> segmento criado.",
            "tcp"
        );

    }

    else{

        setEvent(
            "UDP criou um datagrama",
            "O UDP adiciona as portas e envia os dados sem estabelecer conexão.",
            "🟠",
            "udp"
        );

        logEvent(
            "<b>UDP:</b> datagrama criado.",
            "udp"
        );

    }


    await sleep(5000);


    /* IP */

    activateTimeline(3);

    highlightLayer("layer-ip");


    setEvent(
        "IP adicionou os endereços",
        "O pacote recebe os endereços IP de origem e destino.",
        "🌐"
    );


    logEvent(
        "IP: 192.168.1.10 → 192.168.2.20"
    );


    await sleep(5000);


    /* ETHERNET */

    activateTimeline(4);

    highlightLayer("layer-ethernet");


    setEvent(
        "Ethernet preparou o quadro",
        "A camada de enlace prepara os dados para atravessar a rede local.",
        "🔗"
    );


    logEvent(
        "Ethernet: quadro preparado."
    );


    await sleep(5000);


    /* REDE */

    activateTimeline(5);


    setEvent(
        "Pacote entrou na rede",
        "Observe o pacote atravessando switches e roteadores.",
        "📡"
    );


    logEvent(
        "Pacote transmitido pela rede."
    );


    const packet =
        createPacket(data);


    document
        .querySelectorAll(".link")
        .forEach((link,index)=>{

            setTimeout(
                ()=>{
                    link.classList.add("active");
                },
                index*180
            );

        });


    await sleep(700);


    /* =====================================================
       PERDA
    ===================================================== */

    if(lossArmed){

        lossArmed=false;


        packet.classList.add("lost");


        setEvent(
            "💥 PACOTE PERDIDO",
            protocol==="TCP"
            ?
            "O segmento foi perdido. O TCP não pode concluir a entrega porque não recebeu confirmação."
            :
            "O datagrama foi perdido. O UDP não possui retransmissão automática.",
            "💥",
            "loss"
        );


        logEvent(

            protocol==="TCP"
            ?
            "<b>TCP:</b> segmento perdido. A transmissão NÃO foi concluída."
            :
            "<b>UDP:</b> datagrama perdido. Não haverá retransmissão automática.",

            "loss"

        );


        packet.style.transform =
            "scale(1.35) rotate(10deg)";


        await sleep(2500);


        packet.remove();


        if(protocol==="TCP"){

            await tcpRetransmission(data);

        }

        else{

            clearTimeline();


            controlInfo.innerHTML =
                "<b>UDP:</b> transmissão encerrada com perda.<br><br>" +
                "Parte dos dados pode não ter chegado.";


            logEvent(
                "<b>UDP finalizado:</b> dados podem estar incompletos.",
                "loss"
            );

        }


        return;

    }


    /* =====================================================
       CHEGADA
    ===================================================== */

    await movePacket(
        packet,
        forwardPositions,
        data
    );


    activateTimeline(6);

    highlightLayer("layer-app");


    setEvent(
        "Servidor recebeu os dados",
        protocol==="TCP"
        ?
        "O servidor recebeu os dados e poderá confirmar através de ACK."
        :
        "O servidor recebeu o datagrama. O UDP não exige confirmação.",
        "🖥️",
        protocol==="TCP" ? "tcp" : "udp"
    );


    logEvent(

        protocol==="TCP"
        ?
        "Servidor recebeu os dados."
        :
        "Servidor recebeu o datagrama UDP.",

        "success"

    );


    await sleep(5000);


    if(protocol==="TCP"){

        await sendACK(data);

    }

    else{

        packet.remove();

        finishTimeline();


        controlInfo.innerHTML =
            "<b>UDP concluído.</b><br><br>" +
            "Não houve ACK nem retransmissão.";


        logEvent(
            "<b>UDP:</b> transmissão concluída sem confirmação.",
            "udp"
        );

    }


    sequence += payload.length;

}


/* =========================================================
   ACK
========================================================= */

async function sendACK(original){

    clearLinks();


    const ackData = {

        type:"ACK",

        protocol:"TCP",

        source:"Servidor B",

        destination:"Computador A",

        sourcePort:80,

        destinationPort:49152,

        sequence:5000,

        ack:
            original.sequence +
            original.payload.length,

        payload:""

    };


    highlightLayer("layer-transport");


    setEvent(
        "Servidor enviou ACK",
        "O ACK confirma ao computador A que os dados foram recebidos.",
        "✔",
        "tcp"
    );


    logEvent(
        "<b>ACK enviado:</b> confirmação de recebimento.",
        "success"
    );


    await sleep(5000);


    const packet =
        createPacket(ackData);


    packet.classList.add("ack");


    packet.style.left="90%";

    packet.style.top="55%";


    await movePacket(

        packet,

        [
            {
                left:"90%",
                top:"55%"
            },

            {
                left:"72%",
                top:"55%"
            },

            {
                left:"57%",
                top:"55%"
            },

            {
                left:"40%",
                top:"55%"
            },

            {
                left:"27%",
                top:"55%"
            },

            {
                left:"7%",
                top:"55%"
            }
        ],

        ackData

    );


    finishTimeline();


    setEvent(
        "✔ TCP: transmissão confirmada",
        "O ACK chegou ao computador A. Os dados foram confirmados.",
        "✔",
        "tcp"
    );


    controlInfo.innerHTML =
        "<b>TCP concluído.</b><br><br>" +
        "Dados recebidos e confirmados pelo ACK.";


    logEvent(
        "<b>TCP concluído:</b> dados confirmados.",
        "success"
    );


    await sleep(1200);


    packet.remove();

}


/* =========================================================
   RETRANSMISSÃO TCP
========================================================= */

async function tcpRetransmission(original){

    clearTimeline();


    setEvent(
        "⏳ TCP aguardando confirmação",
        "O segmento foi perdido e não houve ACK.",
        "⏳",
        "tcp"
    );


    controlInfo.innerHTML =
        "<b>TCP detectou a perda.</b><br><br>" +
        "O segmento não foi confirmado.";


    logEvent(
        "<b>TCP:</b> nenhum ACK recebido.",
        "loss"
    );


    await sleep(5000);


    setEvent(
        "🔄 TCP retransmitindo",
        "O TCP envia novamente os dados que não foram confirmados.",
        "🔄",
        "tcp"
    );


    logEvent(
        "<b>Retransmissão TCP:</b> enviando novamente.",
        "tcp"
    );


    activateTimeline(2);

    highlightLayer("layer-transport");


    await sleep(5000);


    const retry =
        createPacket(original);


    retry.style.left="7%";

    retry.style.top="42%";


    setEvent(
        "Segmento retransmitido",
        "O mesmo dado está novamente atravessando a rede.",
        "📤",
        "tcp"
    );


    await movePacket(
        retry,
        forwardPositions,
        original
    );


    finishTimeline();


    setEvent(
        "Servidor recebeu a retransmissão",
        "O servidor recebeu o dado que havia sido perdido.",
        "🖥️",
        "tcp"
    );


    logEvent(
        "<b>TCP:</b> retransmissão chegou ao servidor.",
        "success"
    );


    await sleep(5000);


    await sendACK(original);


    retry.remove();

}


/* =========================================================
   SIMULAR PERDA
========================================================= */

function armLoss(){

    lossArmed=true;


    setEvent(
        "💥 Perda armada",
        "O próximo pacote de dados será perdido durante a transmissão.",
        "💥",
        "loss"
    );


    controlInfo.innerHTML =
        "<b>Perda ativada.</b><br><br>" +
        "Agora clique em <b>Enviar dados</b>.";


    logEvent(
        "<b>Experimento:</b> próxima transmissão será perdida.",
        "loss"
    );

}


/* =========================================================
   ACK MANUAL
========================================================= */

async function sendACKManually(){

    if(protocol!=="TCP"){

        setEvent(
            "ACK pertence ao experimento TCP",
            "O UDP não utiliza ACK como mecanismo próprio de confirmação.",
            "⚠️"
        );

        return;

    }


    if(!currentPacket){

        setEvent(
            "Nenhum pacote disponível",
            "Envie uma mensagem primeiro.",
            "⚠️"
        );

        return;

    }


    await sendACK(currentPacket);

}


/* =========================================================
   DUPLICAÇÃO
========================================================= */

function duplicatePacket(){

    if(!currentPacket){

        setEvent(
            "Nenhum pacote disponível",
            "Envie uma mensagem primeiro.",
            "⚠️"
        );

        return;

    }


    const p =
        createPacket(currentPacket);


    p.style.left="45%";

    p.style.top="30%";

    p.style.transform="scale(1.15)";


    setEvent(
        "📋 Pacote duplicado",
        "Uma segunda cópia visual do pacote foi criada.",
        "📋"
    );


    logEvent(
        "<b>Experimento:</b> pacote duplicado."
    );

}


/* =========================================================
   PRÓXIMA ETAPA
========================================================= */

function stepSimulation(){

    const active =
        document.querySelector(
            ".timeline-item.active"
        );


    let n=1;


    if(active){

        n =
            parseInt(
                active.id.replace("t","")
            ) + 1;


        if(n>6){

            n=1;

        }

    }


    activateTimeline(n);


    const names = [

        "Aplicação",

        "Transporte",

        "IP",

        "Ethernet",

        "Rede",

        "Servidor"

    ];


    const layers = [

        "layer-app",

        "layer-transport",

        "layer-ip",

        "layer-ethernet",

        "layer-ethernet",

        "layer-app"

    ];


    highlightLayer(
        layers[n-1]
    );


    setEvent(
        "Etapa " + n + " — " + names[n-1],
        "Observe a responsabilidade dessa etapa na comunicação.",
        "➡️"
    );


    logEvent(
        "Etapa manual: <b>" +
        names[n-1] +
        "</b>."
    );

}


/* =========================================================
   MODO AUTOMÁTICO
========================================================= */

async function toggleAuto(){

    if(autoRunning){

        autoRunning=false;

        document
            .getElementById("autoButton")
            .textContent =
            "▶ Modo automático";

        setEvent(
            "Modo automático pausado",
            "Você pode continuar manualmente.",
            "⏸️"
        );

        return;

    }


    autoRunning=true;


    document
        .getElementById("autoButton")
        .textContent =
        "⏸ Pausar automático";


    setEvent(
        "Modo automático iniciado",
        "Observe a sequência completa da comunicação.",
        "▶️"
    );


    if(protocol==="TCP" && !tcpConnected){

        await startTCP();

    }


    if(autoRunning){

        await sendMessage();

    }


    autoRunning=false;


    document
        .getElementById("autoButton")
        .textContent =
        "▶ Modo automático";

}


/* =========================================================
   REINICIAR
========================================================= */

function resetSimulation(){

    protocol="TCP";

    tcpConnected=false;

    lossArmed=false;

    sequence=1000;

    currentPacket=null;

    autoRunning=false;


    document
        .getElementById("tcpButton")
        .classList.add("active");


    document
        .getElementById("udpButton")
        .classList.remove("active");


    transportLabel.textContent="TCP";


    document
        .querySelectorAll(".packet")
        .forEach(packet=>{
            packet.remove();
        });


    clearTimeline();

    clearLinks();


    document
        .querySelectorAll(".layer")
        .forEach(layer=>{
            layer.classList.remove("active");
        });


    inspector.innerHTML=`

        <div class="empty">

            Clique em um pacote durante
            a simulação para examinar
            seus cabeçalhos.

        </div>

    `;


    messageLog.innerHTML=`

        <div class="log">

            Laboratório reiniciado.

        </div>

    `;


    progressBar.style.width="0%";


    document
        .getElementById("autoButton")
        .textContent =
        "▶ Modo automático";


    setEvent(
        "Laboratório pronto",
        "Escolha TCP ou UDP e inicie um experimento.",
        "📡"
    );


    controlInfo.innerHTML =
        "<b>TCP selecionado.</b><br><br>" +
        "Faça o handshake ou use o modo automático.";

}


/* =========================================================
   INICIALIZAÇÃO
========================================================= */

setProtocol("TCP");

</script>

</body>
</html>
