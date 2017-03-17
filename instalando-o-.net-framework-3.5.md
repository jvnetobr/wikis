# Erro de instalação do.NET Framework 3.5: 0x800F0906, 0x800F081F ou 0x800F0907

### Para definir a configuração de Diretiva de Grupo, execute estas etapas:

#### 1. Faça uma cópia da imagem ISO de acordo com a versão instalada, do servidor SRV-WDS-01 (10.10.10.174) para a estação e faça a montagem.

#### 2. Inicie o Editor de Diretiva de Grupo Local ou o Console de Gerenciamento de Diretiva de Grupo, acessando a aplicação "executar", e em seguida "gpedit.msc".

![Capturar01](/uploads/b61f18f6762302c78ab085504b233b8e/Capturar01.PNG)

#### 3. Expanda Configuração do Computador, expanda Modelos Administrativose, em seguida, selecione o Sistema

![Capturar02](/uploads/89c4e8b99964a86b708488af3f6b6b4e/Capturar02.PNG)

#### 4. Abra a configuração de Diretiva de Grupo Especificar configurações para instalação de componente opcional e reparo de componente e selecione ativado.

![Capturar03](/uploads/a48098da0fae8b683eaad9e16514f5b4/Capturar03.PNG)

#### 5. Aponte o "caminho do arquivo de origem alternativa" para a pasta de sources\sxs ISO de ISO e clique em OK..

#### 6. Execute o comando gpupdate /force.

#### 7. Adicione o recurso do .NET framework ou instale a partir do instalador offline.