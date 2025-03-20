<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Desafio - Análise de Logs, Redes TCP/IP e OSINT</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <style>
    /* Estilos Gerais */
    body {
      font-family: 'Arial', sans-serif;
      background-color: #eef1f7;
      margin: 0;
      padding: 0;
      text-align: center;
    }
    
    /* Cabeçalho do evento */
    .event-header {
      display: flex;
      align-items: center;
      justify-content: center;
      margin-top: 20px;
    }
    .event-header img {
      height: 60px; /* Ajuste conforme desejar */
      margin: 0 15px;
    }
    .event-title {
      margin-top: 10px;
      font-size: 28px;
      font-weight: bold;
      color: #003f7f;
      text-transform: uppercase;
    }
    .event-subtitle {
      font-size: 14px;
      color: #333;
      margin-top: 5px;
    }
    
    /* Título de cada módulo */
    .title {
      font-size: 28px;
      font-weight: bold;
      margin: 30px 0 10px;
      color: #003f7f;
      text-transform: uppercase;
    }
    
    /* Container dos módulos */
    .container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      padding: 40px;
      max-width: 940px; /* Limita para no máximo 3 boxes por linha */
      margin: 0 auto;
    }
    
    /* Estilos dos Retângulos (Boxes) */
    .box {
      width: 280px;
      height: 140px;
      background: linear-gradient(135deg, #0057A6, #003f7f);
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: flex-end;
      font-size: 18px;
      font-weight: bold;
      color: white;
      cursor: pointer;
      border-radius: 12px;
      padding: 15px;
      position: relative;
      transition: transform 0.3s ease, background 0.3s;
      box-shadow: 4px 4px 10px rgba(0,0,0,0.2);
      text-align: right;
    }
    .box:hover {
      transform: scale(1.1);
    }
    .box.correct {
      background: #28a745 !important;
    }
    
    /* Informações dentro de cada box */
    .lab-name {
      position: absolute;
      top: 10px;
      right: 15px;
      font-size: 14px;
      font-weight: bold;
    }
    .points {
      position: absolute;
      top: 10px;
      left: 15px;
      font-size: 14px;
      font-weight: bold;
      color: yellow;
    }
    .separator {
      width: 100%;
      height: 1px;
      background-color: white;
      margin-top: 35px;
    }
    .challenge-name {
      width: 100%;
      text-align: right;
      padding-right: 15px;
      margin-top: 10px;
    }
    
    /* Estilos do Modal */
    .overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      z-index: 999;
    }
    .modal {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      padding: 25px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
      z-index: 1000;
      border-radius: 12px;
      width: 450px;
      max-width: 90%;
      text-align: left;
      animation: fadeIn 0.3s ease-in-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translate(-50%, -55%); }
      to { opacity: 1; transform: translate(-50%, -50%); }
    }
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 2px solid #0057A6;
      padding-bottom: 8px;
      margin-bottom: 15px;
    }
    .modal-title {
      font-size: 20px;
      font-weight: bold;
      color: #0057A6;
    }
    .close-btn {
      font-size: 20px;
      font-weight: bold;
      cursor: pointer;
      color: #666;
      padding: 5px;
    }
    .close-btn:hover {
      color: #333;
    }
    .modal-content {
      font-size: 16px;
      line-height: 1.6;
      text-align: left;
    }
    .input-container, .ok-btn {
      width: calc(100% - 30px);
      padding: 12px;
      border-radius: 6px;
      font-size: 16px;
      border: 1px solid #ccc;
      display: block;
      margin: 10px auto;
      text-align: center;
      max-width: 400px;
    }
    .ok-btn {
      background: linear-gradient(135deg, #0073e6, #0057A6);
      color: white;
      border: none;
      cursor: pointer;
      font-size: 16px;
      font-weight: bold;
      text-transform: uppercase;
      transition: background 0.3s;
    }
    .ok-btn:hover {
      background: #003f7f;
    }
    .feedback {
      font-size: 14px;
      font-weight: bold;
      color: red;
      text-align: center;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <!-- Cabeçalho do evento com as imagens -->
  <div class="event-header">
    <img src="firjan_senai_logo.png" alt="Firjan SENAI" />
    <img src="worldskills_logo.png" alt="WorldSkills" />
  </div>
  <!-- Título do evento e subtítulo -->
  <div class="event-title">ESTADUAL WORLDsKILLS RJ</div>
  <div class="event-subtitle">SENAI MARACANÃ</div>
  
  <!-- Módulo A1 - Análise de Logs (6 questões) -->
  <div class="title">Módulo A1 - Análise de Logs</div>
  <div class="container">
    <div class="box" data-question="Quantos endereços IPs estão envolvidos no log?" data-answer="setenta">
      <div class="points">5 pontos</div>
      <div class="lab-name">Intro</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs I</div>
    </div>
    <div class="box" data-question="Qual o endereço IP do atacante?" data-answer="200.200.200.200">
      <div class="points">5 pontos</div>
      <div class="lab-name">Ataque</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs II</div>
    </div>
    <div class="box" data-question="Quando ocorreu a primeira requisição no log?" data-answer="21/04/2018:13:15:30">
      <div class="points">5 pontos</div>
      <div class="lab-name">Primeira Req.</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs III</div>
    </div>
    <div class="box" data-question="Qual código de status aparece com mais frequência no log?" data-answer="200">
      <div class="points">5 pontos</div>
      <div class="lab-name">Status</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs IV</div>
    </div>
    <div class="box" data-question="Qual foi o horário da última requisição registrada no log?" data-answer="23/04/2018:23:59:59">
      <div class="points">5 pontos</div>
      <div class="lab-name">Última Req.</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs V</div>
    </div>
    <div class="box" data-question="Qual o total de requisições registradas no log?" data-answer="350">
      <div class="points">5 pontos</div>
      <div class="lab-name">Total Req.</div>
      <div class="separator"></div>
      <div class="challenge-name">Análise de Logs VI</div>
    </div>
  </div>

  <!-- Módulo A2 - Redes TCP/IP (6 questões) -->
  <div class="title">Módulo A2 - Redes TCP/IP</div>
  <div class="container">
    <div class="box" data-question="Qual é a porta padrão do protocolo FTP?" data-answer="21">
      <div class="points">5 pontos</div>
      <div class="lab-name">Portas</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP I</div>
    </div>
    <div class="box" data-question="Qual protocolo é usado para resolução de nomes de domínio?" data-answer="DNS">
      <div class="points">5 pontos</div>
      <div class="lab-name">Domínios</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP II</div>
    </div>
    <div class="box" data-question="O que significa a sigla NAT?" data-answer="Network Address Translation">
      <div class="points">5 pontos</div>
      <div class="lab-name">Endereçamento</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP III</div>
    </div>
    <div class="box" data-question="Qual protocolo de roteamento dinâmico é utilizado para redes internas?" data-answer="RIP">
      <div class="points">5 pontos</div>
      <div class="lab-name">Roteamento</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP IV</div>
    </div>
    <div class="box" data-question="Qual comando é utilizado para visualizar tabelas de roteamento no Linux?" data-answer="route">
      <div class="points">5 pontos</div>
      <div class="lab-name">Comandos</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP V</div>
    </div>
    <div class="box" data-question="Qual é a porta padrão do protocolo HTTPS?" data-answer="443">
      <div class="points">5 pontos</div>
      <div class="lab-name">Segurança</div>
      <div class="separator"></div>
      <div class="challenge-name">Redes TCP/IP VI</div>
    </div>
  </div>

  <!-- Módulo A3 - OSINT (10 questões) -->
  <div class="title">Módulo A3 - OSINT</div>
  <div class="container">
    <div class="box" data-question="O que significa a sigla OSINT?" data-answer="Open Source Intelligence">
      <div class="points">5 pontos</div>
      <div class="lab-name">Conceito</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT I</div>
    </div>
    <div class="box" data-question="Qual ferramenta é amplamente usada para coleta de informações em redes sociais?" data-answer="Maltego">
      <div class="points">5 pontos</div>
      <div class="lab-name">Ferramentas</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT II</div>
    </div>
    <div class="box" data-question="Qual mecanismo de busca especializado é usado para encontrar dispositivos conectados à internet?" data-answer="Shodan">
      <div class="points">5 pontos</div>
      <div class="lab-name">Busca</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT III</div>
    </div>
    <div class="box" data-question="Qual técnica OSINT é usada para rastrear imagens na web?" data-answer="Reverse Image Search">
      <div class="points">5 pontos</div>
      <div class="lab-name">Imagens</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT IV</div>
    </div>
    <div class="box" data-question="Qual ferramenta pode ser usada para pesquisar informações de domínio?" data-answer="Whois">
      <div class="points">5 pontos</div>
      <div class="lab-name">Domínios</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT V</div>
    </div>
    <div class="box" data-question="Qual site é frequentemente usado para verificar vazamentos de credenciais?" data-answer="Have I Been Pwned">
      <div class="points">5 pontos</div>
      <div class="lab-name">Credenciais</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT VI</div>
    </div>
    <div class="box" data-question="Qual técnica permite encontrar e-mails associados a um domínio?" data-answer="Email Harvesting">
      <div class="points">5 pontos</div>
      <div class="lab-name">Emails</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT VII</div>
    </div>
    <div class="box" data-question="Qual ferramenta é utilizada para monitorar a web em tempo real?" data-answer="Google Alerts">
      <div class="points">5 pontos</div>
      <div class="lab-name">Monitoramento</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT VIII</div>
    </div>
    <div class="box" data-question="Qual é a sigla para a técnica de análise de metadados de arquivos?" data-answer="EXIF">
      <div class="points">5 pontos</div>
      <div class="lab-name">Metadados</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT IX</div>
    </div>
    <div class="box" data-question="Qual é um exemplo de fonte pública para OSINT?" data-answer="Redes Sociais">
      <div class="points">5 pontos</div>
      <div class="lab-name">Fontes</div>
      <div class="separator"></div>
      <div class="challenge-name">OSINT X</div>
    </div>
  </div>

  <!-- Modal Padrão para Inserção de Resposta (estrutura solicitada) -->
  <div class="overlay"></div>
  <div class="modal">
    <div class="modal-header">
      <span class="modal-title">Desafio</span>
      <span class="close-btn">&times;</span>
    </div>
    <div class="modal-content">
      <p>Analise o arquivo de log e responda:</p>
      <p id="modal-question"></p>
      <p><strong>Exemplo:</strong> vinteecinco</p>
      <p><strong>Arquivo para análise:</strong> ACCESS.LOG</p>
      <p><a href="http://www.businesscorp.com.br/access.log" target="_blank">http://www.businesscorp.com.br/access.log</a></p>
      <hr>
      <input type="text" id="answer" class="input-container" placeholder="Digite sua resposta...">
      <button id="submit" class="ok-btn">OK</button>
      <p id="feedback" class="feedback"></p>
    </div>
  </div>

  <script>
    let currentBox;
    $(".box").click(function() {
      currentBox = $(this);
      let question = currentBox.attr("data-question");
      $("#modal-question").text(question);
      $("#answer").val("");
      $("#feedback").text("");
      $(".overlay, .modal").fadeIn(200);
    });
    $(".close-btn, .overlay").click(function() {
      $(".overlay, .modal").fadeOut(200);
    });
    $("#submit").click(function() {
      let userAnswer = $("#answer").val().trim().toLowerCase();
      let correctAnswer = currentBox.attr("data-answer").toLowerCase();
      if (userAnswer === correctAnswer) {
        currentBox.addClass("correct");
        $(".overlay, .modal").fadeOut(200);
      } else {
        $("#feedback").text("Resposta incorreta");
      }
    });
  </script>
</body>
</html>
