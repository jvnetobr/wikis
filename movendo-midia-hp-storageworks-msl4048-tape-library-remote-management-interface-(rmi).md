# Movendo mídia - HP StorageWorks MSL4048 Tape Library - Remote Management Interface (RMI)

### Visão geral

A interface de gerenciamento remoto (RMI) permite monitorar e controlar um dispositivo a partir de qualquer terminal conectado à rede ou através da World Wide Web (WWW). O RMI hospeda um site dedicado e protegido na Internet que exibe uma representação gráfica de um dispositivo.

NOTA: Verifique as telas de Ajuda no "RMI" para obter informações adicionais. As páginas de ajuda são atualizadas com a maioria das atualizações de firmware e muitas vezes contêm detalhes técnicos que não estão contidos neste documento. Para acessar a ajuda do "RMI", clique em Ajuda no lado direito do banner da página da Web, conforme mostrado em Obtendo ajuda.

### Login

Para efetuar login, selecione o tipo de conta, insira uma senha, se necessário, e clique em Entrar.

![rmi_login_page](/uploads/ae6a96ef443f4f05ef1c7dbbe836bea4/rmi_login_page.jpg)

Os Tipos de Conta são:

* Usuário: Nenhuma senha é necessária (deixe a caixa de senha em branco).

* Administrador: A senha do administrador é necessária. A mesma senha de administrador é usada para o "RMI" e o "OCP" (Operator Control Panel). Não há uma senha de administrador padrão; A senha de administrador deve ser definida com o "OCP" antes de poder ser usada com o "RMI". Se a senha de administrador for perdida, entre em contato com a HP para gerar uma senha temporária que conceda acesso de administrador.

* Serviço: (o acesso a este nível é feito somente pelo pessoal do Serviço HP). A senha do serviço é definida na fábrica. A mesma senha de serviço é usada para o RMI eo OCP.

O login do usuário fornece acesso às opções Identidade e Status, mas não às opções Configuração, Operações e Suporte. O nível de administrador fornece acesso a todas as telas, exceto para as telas de configuração de log e de serviço HP.

NOTA: Por predefinição, a palavra-passe de administrador é unset; Todos os dígitos são nulos. A senha de administrador deve ser definida a partir do OCP para proteger as funções de administrador no "OCP" e habilitar as funções de administrador no "RMI".

### Movendo mídia

Use a página "Operations: Move Media page" para mover os cartuchos de fita dentro do dispositivo.

NOTA: A movimentação manual da mídia pode interferir nas operações do software de backup. Certifique-se de que os backups estão completos antes de mover a mídia.

![move_media_page_01](/uploads/3f90d840c5ba3f1595a0711250cfc82e/move_media_page_01.jpg)

Para mover uma fita, selecione a fonte e o destino e clique no botão "Move" no centro da tela para iniciar a movimentação.

### Atualizando o inventário de mídia atual

Use a página "Operations: Inventory page" para que o dispositivo revise as fitas para atualizar o inventário de mídia.

![inventory_page](/uploads/1f2641be03f09894bcac261b30e967a5/inventory_page.jpg)

### Liberar e substituir as revistas

Use a página "Operations: Magazine page" para liberar o compartimento direito ou esquerdo. Quando você clicar em "Release", o dispositivo desbloqueará o compartimento e exibirá "Left Magazine Unlocked" ou "Right Magazine Unlocked" na tela "OCP". O compartimento não se move até que você o puxe para fora do dispositivo. Se você não remover o compartimento dentro de alguns segundos, o dispositivo o travará. Quando você substituir a revista, o dispositivo inventariará os cartuchos de fita da revista.

![magazine_page](/uploads/08c783c2755ef484e043baf5f7b1a098/magazine_page.jpg)
