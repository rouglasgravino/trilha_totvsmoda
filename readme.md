Trilha de Estudos Interativa - Preparatório TOTVS Moda
Descrição Curta (para o campo "About" do GitHub)
Uma página web interativa e responsiva que organiza a playlist de vídeos do "Preparatório Certificação ASSOCIATE - TOTVS Moda" em uma trilha de estudos lógica. Inclui acompanhamento de progresso individual salvo localmente no navegador.

README.md
🚀 Trilha de Estudos Interativa - Preparatório TOTVS Moda
Bem-vindo à Trilha de Estudos Interativa para a Certificação TOTVS Moda! Este projeto organiza a playlist oficial de vídeos de preparação da TOTVS em uma página web amigável, estruturada em módulos lógicos que seguem o fluxo de trabalho de um sistema de gestão (ERP).

O objetivo é facilitar a jornada de aprendizado, permitindo que o usuário estude de forma progressiva, do básico ao avançado, e acompanhe seu próprio progresso.


✨ Funcionalidades
Organização Modular: Os vídeos são agrupados em módulos temáticos (Configurações, Vendas, Financeiro, etc.) para um aprendizado estruturado.

Interface Interativa: Cada módulo é um "acordeão" (accordion) que pode ser expandido ou recolhido, mantendo a interface limpa.

Acompanhamento de Progresso: Marque os vídeos que já assistiu com checkboxes e visualize seu avanço em uma barra de progresso.

Progresso Salvo no Navegador: Seu progresso é salvo localmente (localStorage), permitindo que você feche a página e continue de onde parou. Ideal para ser compartilhado em rede, pois o progresso é individual.

Design Responsivo: A página se adapta a diferentes tamanhos de tela, funcionando bem em desktops, tablets e celulares.

Contador de Vídeos: Um contador exibe o número total de vídeos disponíveis na trilha.

🛠️ Como Utilizar
Não é necessária nenhuma instalação ou configuração complexa.


Abra o arquivo index.html (ou o nome que você deu ao arquivo HTML) em qualquer navegador de internet.

Pronto! Comece seus estudos.

⚙️ Ferramenta: Script para Extração de Vídeos da Playlist
Para manter a trilha de estudos atualizada, você pode usar o script abaixo para extrair todos os títulos e links de uma playlist do YouTube diretamente para um arquivo CSV.

Instruções:

Abra a página da playlist do YouTube no seu navegador.

Pressione F12 para abrir as Ferramentas de Desenvolvedor.

Clique na aba Console.

O console do navegador pode bloquear a colagem de código. Se isso acontecer, digite allow pasting e pressione Enter.

Copie todo o script abaixo, cole no console e pressione Enter.

Aguarde! O script irá rolar a página até o final para carregar todos os vídeos e, em seguida, fará o download automático do arquivo playlist_do_youtube.csv.

async function extrairVideosDaPagina() {
    console.clear();
    console.log("%c🚀 Iniciando o extrator de vídeos (v3)...", "font-size: 16px; font-weight: bold; color: blue;");

    // 1. Rolar até ao fim da página para carregar todos os vídeos
    const rolarParaOFim = () => {
        return new Promise(resolve => {
            let totalHeight = 0;
            let distance = 200;
            const timer = setInterval(() => {
                const scrollHeight = document.body.scrollHeight;
                window.scrollBy(0, distance);
                totalHeight += distance;

                // Se não houver mais para rolar, termina
                if (totalHeight >= scrollHeight) {
                    clearInterval(timer);
                    // Damos um segundo extra para o YouTube carregar os últimos elementos
                    setTimeout(() => resolve(), 1000);
                }
            }, 100);
        });
    };

    console.log("⏳ Rolando a página para carregar todos os vídeos. Por favor, aguarde...");
    await rolarParaOFim();
    console.log("%c✅ Todos os vídeos foram carregados. Extraindo dados...", "font-size: 14px; color: green;");

    // 2. Extrair os Títulos e Links com um seletor aprimorado
    const elementosVideo = document.querySelectorAll('ytd-playlist-video-renderer #video-title');

    if (elementosVideo.length === 0) {
        console.error("%c❌ ERRO: Nenhum vídeo foi encontrado. O YouTube pode ter atualizado a estrutura da página.", "font-size: 16px; font-weight: bold;");
        return; // Termina a execução se não encontrar nada
    }

    const videos = [];
    elementosVideo.forEach(el => {
        const url = new URL(el.href);
        const params = new URLSearchParams(url.search);
        params.delete('list');
        params.delete('index');
        params.delete('pp');
        url.search = params.toString();
        
        videos.push({
            titulo: el.title,
            link: url.href
        });
    });

    // 3. Apresentar os resultados na consola
    console.log(`\n\n%c--- ${videos.length} VÍDEOS ENCONTRADOS ---`, "font-size: 18px; font-weight: bold; color: red;");
    console.table(videos);
    console.log("%c--- FIM DA LISTA ---", "font-size: 14px; font-weight: bold;");

    // 4. Criar e descarregar um ficheiro CSV
    let csvContent = "data:text/csv;charset=utf-8,Título,URL\n";
    videos.forEach(row => {
        const titulo = `"${row.titulo.replace(/"/g, '""')}"`;
        csvContent += `${titulo},"${row.link}"\n`;
    });

    const encodedUri = encodeURI(csvContent);
    const linkElement = document.createElement("a");
    linkElement.setAttribute("href", encodedUri);
    linkElement.setAttribute("download", "playlist_do_youtube.csv");
    document.body.appendChild(linkElement);
    linkElement.click();
    document.body.removeChild(linkElement);

    console.log("%c🎉 Arquivo 'playlist_do_youtube.csv' baixado com sucesso!", "font-size: 16px; color: blue;");
}

extrairVideosDaPagina();

💻 Tecnologias Utilizadas
Este projeto foi construído com tecnologias web padrões, sem a necessidade de um back-end.

HTML5: Para a estrutura da página.

Tailwind CSS: Para a estilização rápida e responsiva.

JavaScript (Vanilla): Para toda a interatividade, incluindo a lógica do acordeão e o sistema de acompanhamento de progresso.



📄 Licença
Este projeto está sob a licença MIT. Veja o arquivo LICENSE.md para mais detalhes.