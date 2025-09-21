Este documento é um tutorial sobre a fabricação de placas de circuito impresso (PCB) usando uma máquina CNC. Ele aborda o processo completo, desde a exportação dos arquivos de design até a usinagem e finalização da placa.

---

### **Introdução**

[cite_start]Este tutorial explica os passos necessários para fabricar uma placa de circuito por meio de usinagem CNC[cite: 4]. [cite_start]O processo começa com o design da placa em um software como o KiCad, seguido pela geração do código G usando o FlatCAM, e finaliza com a fabricação da placa usando o software Candle para controlar a máquina CNC[cite: 5].

[cite_start]Para este tutorial, a máquina CNC TwoTrees TTC-3018 S é utilizada[cite: 6]. [cite_start]É recomendado ler o tutorial completo antes de começar, pois há avisos importantes[cite: 7].

### **Exportando sua Placa de Circuito**

[cite_start]O foco é a fabricação, não o projeto da placa, então o tutorial assume que você já tem um projeto pronto no KiCad[cite: 9]. [cite_start]É importante que a placa tenha sido desenhada na parte de trás (o lado azul no KiCad) para que a orientação de fabricação esteja correta[cite: 10].

[cite_start]O primeiro passo é exportar os arquivos **Gerber (.gbr)** e **Drill (.drl)**, que contêm as trilhas e os furos para os componentes, respectivamente[cite: 11].

#### **Exportando Gerber no KiCad**

Com a PCB aberta no KiCad, siga estes passos:
1.  [cite_start]Vá em **File > Fabrication outputs > Gerbers (.gbr)**[cite: 13, 14].
2.  [cite_start]Na janela que se abre, selecione as camadas **B.Cu** (área de cobre) e **Edge.Cuts** (corte do contorno) em **Include Layers**[cite: 20, 21].
3.  [cite_start]Clique em **"Plot"**[cite: 21].
4.  [cite_start]Não feche a janela, pois ela será usada para gerar os arquivos dos furos[cite: 24].

[cite_start]Este tutorial é para placas de circuito com apenas uma camada de cobre[cite: 22].

#### **Exportando Arquivos de Furos no KiCad**

[cite_start]Para exportar os furos, clique em **"Generate Drill Files..."** na mesma janela de plotagem[cite: 28]. [cite_start]Na próxima janela, escolha as opções de unidades e formato de arquivo e pressione **"Generate Drill File"**[cite: 106].

[cite_start]Após esses passos, você terá todos os arquivos necessários para a fabricação, e o KiCad não será mais necessário[cite: 154, 155].

### **Configurando e Utilizando o FlatCAM**

[cite_start]O software FlatCAM beta 8.994 é usado para gerar o código G[cite: 158].

1.  [cite_start]Abra o FlatCAM e vá em **File > Open Gerber**[cite: 159].
2.  [cite_start]Selecione e abra os arquivos das trilhas e bordas da placa: `nome-Cu.gbr` e `nome-Edge_Cuts.gbr`[cite: 159].
3.  [cite_start]Em seguida, vá em **File > Open > Open Excellon** e abra o arquivo `nome-PTH.drl` para os furos[cite: 174, 175].
4.  [cite_start]Arraste o mouse sobre os objetos e clique em **Edit > Move to origin** (Shift + O) para mover a placa para a origem[cite: 180, 181].

#### **Configurando Parâmetros**

1.  [cite_start]Vá em **Edit > Preferences**[cite: 238].
2.  [cite_start]Na aba **"General"**, defina a unidade como **mm**[cite: 238, 241].
3.  [cite_start]Na aba **"Excellon"**, mude as unidades para **mm**[cite: 250, 257].
4.  Na aba **"Geometry"**, faça os seguintes ajustes:
    * [cite_start]**Cut Z** (profundidade de corte): $-0.25$[cite: 262].
    * [cite_start]Ative **multi-depth** e defina **depth/pass** como $0.125$[cite: 263].
    * [cite_start]**Travel Z** (altura de viagem da ferramenta): $1$[cite: 264].
    * [cite_start]**Spindle speed**: $9000$[cite: 264].
5.  [cite_start]Clique em **"Apply"** para salvar as mudanças[cite: 352].

#### **Geração do Código G**

---

#### **Isolation Routing (Trilhas)**

Para gerar o código G das trilhas:
1.  [cite_start]Clique duas vezes no arquivo `nome-Cu.gbr` e selecione **"Isolation Routing"**[cite: 427, 441].
2.  [cite_start]Em **Isolation Tool**, defina o diâmetro da ferramenta (**Tool Dia**) como $0.2$[cite: 455].
3.  [cite_start]Clique em **"Generate Geometry"**[cite: 455, 489].
4.  [cite_start]Verifique a geometria gerada com zoom para garantir que não há erros[cite: 496].
5.  [cite_start]Em seguida, clique em **"Generate CNCJob object"**[cite: 534].
6.  [cite_start]Salve o código de CNC[cite: 539, 566].

---

#### **Cutout (Recorte da Borda)**

Para gerar o código G do recorte da placa:
1.  [cite_start]Clique duas vezes no arquivo `nome-Edge_Cuts.gbr` e selecione **"Cutout Tool"**[cite: 569, 581].
2.  [cite_start]Em **Cutout PCB**, defina o **Cut Z** como $-1.8000$ e o **Tool Dia** como $1.0000$[cite: 590, 591, 603, 608].
3.  [cite_start]Clique em **"Generate Geometry"**[cite: 620].
4.  [cite_start]Na aba **Project**, clique duas vezes no arquivo `nome-Edge_Cuts.gbr_cutout` e gere o CNCJob[cite: 628, 661].
5.  [cite_start]Salve o código CNC[cite: 666].

---

#### **Furos**

Para gerar o código G dos furos:
1.  [cite_start]Na aba **Project**, clique duas vezes no arquivo `nome-PTH.drl`[cite: 671].
2.  [cite_start]Abra **"Drilling Tool"** e clique em **"Generate CNCJob object"**[cite: 672, 729, 734].
3.  [cite_start]Salve o código CNC[cite: 736].

[cite_start]Com todos os códigos G gerados, o FlatCAM não é mais necessário[cite: 737].

### **Utilizando a Máquina CNC com Candle**

[cite_start]A máquina CNC TwoTrees TTC-3018 S e o software Candle 1.2b serão usados para controlar o processo[cite: 6, 742].

**Avisos Importantes:**
* [cite_start]Sempre verifique se o probe está conectado ao eixo da fresa e à placa antes de qualquer ação para evitar danos à fresadora[cite: 747, 748].
* [cite_start]Sempre garanta que a área de trabalho da máquina é suficiente para o projeto, caso contrário, a máquina pode ser danificada[cite: 749, 750].
* [cite_start]Não inicie a máquina com o probe conectado à fresa[cite: 1446].

#### **Configurando o Candle**

1.  [cite_start]Ligue a máquina CNC e conecte-a ao computador[cite: 751].
2.  [cite_start]Abra o Candle e vá em **Service > Settings**[cite: 760].
3.  [cite_start]Em **"Connection"**, selecione a porta COM à qual a CNC está conectada[cite: 767].

#### **Ferramentas de Usinagem**

Serão usadas duas fresas:
* [cite_start]A **fresa tipo V** para traçar as trilhas da placa[cite: 903].
* [cite_start]A **fresa esférica** para fazer os furos e recortar a placa[cite: 903].

#### **Processo de Fabricação**

[cite_start]Siga a ordem: **Trilhas > Furos > Recorte da Borda**[cite: 1342].

1.  **Trilhas**:
    * [cite_start]Instale a fresa tipo V[cite: 905].
    * [cite_start]Abra o código G das trilhas (`nome-Cu.gbr_iso_combined_cnc`) em **File > Open** no Candle[cite: 908, 919].
    * [cite_start]Mova a fresa para a origem desejada da placa usando a função **Jog**[cite: 944].
    * [cite_start]Zere os eixos XY[cite: 944].
    * [cite_start]Conecte um probe à fresa e o outro à placa[cite: 955].
    * [cite_start]Clique em **"Probe-Z"** para a fresa descer e encontrar a superfície da placa[cite: 957].
    * [cite_start]Clique em **"Zero Z"** para zerar o eixo Z[cite: 957].
    * [cite_start]Para placas empenadas, crie um **Heightmap** clicando em **"Create"** na seção **Heightmap**[cite: 959, 960].
    * [cite_start]Na janela do Heightmap, clique em **"Auto"** para definir a área[cite: 969].
    * [cite_start]Clique em **"Probe"** para iniciar o mapeamento[cite: 1076].
    * [cite_start]Ao finalizar, salve o mapa clicando em **"Open"**[cite: 1188].
    * [cite_start]Na aba Heightmap, clique em **"Edit mode"** e depois em **"Use heightmap"**[cite: 1329].
    * [cite_start]Remova o probe da fresa e clique em **"Send"** para começar a fresar as trilhas[cite: 1330].

2.  **Furos**:
    * [cite_start]Levante o eixo Z para trocar a fresa para a circular de 1 mm[cite: 1337, 1338].
    * [cite_start]Abra o arquivo do código G dos furos (`nome-PTH.drl_cnc`)[cite: 1337].
    * [cite_start]Conecte o probe novamente e realize o probe e zero do eixo Z[cite: 1339]. [cite_start]Não altere a origem XY[cite: 1339].
    * [cite_start]Retire o probe e clique em **"Send"** para começar a perfurar os furos[cite: 1341, 1395, 1397].

3.  **Recorte da Borda**:
    * [cite_start]Abra o código G do recorte da borda (`edge_cuts`)[cite: 1399].
    * [cite_start]Clique em **"Send"** para recortar a placa[cite: 1400].

### **Conclusão**

[cite_start]Após a usinagem, remova a placa, corte as abas que a prendem ao material e use uma esponja de aço (bombril) para remover qualquer excesso de cobre que possa causar curtos-circuitos[cite: 1432]. [cite_start]Verifique a continuidade das trilhas com um multímetro[cite: 1434]. [cite_start]Por fim, solde os componentes e estanhe a placa para evitar corrosão[cite: 1435].

[cite_start]A fabricação de PCBs por CNC oferece alta precisão e pode ser um processo rápido após a configuração inicial[cite: 1442, 1443]. [cite_start]O sucesso depende de ajustes e experimentação com os parâmetros[cite: 1444].

#### **Problemas Comuns e Soluções**

| Problema | Solução |
| :--- | :--- |
| **Trilhas muito finas ou comidas** | [cite_start]Reduza o Cut-Z ou use uma broca com ângulo menor[cite: 1436]. |
| **Furos danificando as ilhas** | [cite_start]Aumente o tamanho das ilhas no design da placa ou use uma broca em vez de uma fresa[cite: 1436]. |
| **Recorte de borda ou furos incompletos** | [cite_start]Aumente o Cut-Z da borda e dos furos[cite: 1436]. |
| **Corte irregular** | [cite_start]Aumente a resolução do mapa de alturas[cite: 1436]. |
| **Fresa não cortando a placa** | [cite_start]Aumente o Cut-Z[cite: 1436]. |
