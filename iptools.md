## Breve descrição de cada ferramenta e exemplos de como elas podem ser utilizadas para diagnóstico de rede:

1. **Hostname**: Exibe o nome do computador na rede.
   - Exemplo: `hostname`
  
2. **Nbtstat**: Exibe estatísticas do protocolo NetBIOS sobre TCP/IP, como conexões e nomes registrados.
   - Exemplo: `nbtstat -a <endereço_IP>`

3. **Ipconfig**: Exibe informações de configuração do IP para todas as interfaces de rede.
   - Exemplo: `ipconfig /all`

4. **Getmac**: Exibe o endereço MAC de uma placa de rede.
   - Exemplo: `getmac /v`

5. **Ping**: Verifica a conectividade com um host remoto.
   - Exemplo: `ping <endereço_IP>` ou `ping <nome_do_host>`

6. **Net**: Exibe ou controla serviços de rede.
   - Exemplo: `net view` para exibir recursos compartilhados em uma rede.

7. **Netstat**: Exibe estatísticas da interface de rede e conexões TCP/IP ativas.
   - Exemplo: `netstat -ano` para exibir todas as conexões e processos associados.

8. **Netsh**: Ferramenta de linha de comando para configuração de rede.
   - Exemplo: `netsh interface ip show config` para exibir configurações IP.

9. **Arp**: Exibe e modifica a tabela ARP do sistema.
   - Exemplo: `arp -a` para exibir a tabela ARP completa.

10. **Tracert**: Rastreia a rota que um pacote IP segue até alcançar um host.
    - Exemplo: `tracert <endereço_IP>`

11. **Route**: Exibe e manipula a tabela de roteamento IP do sistema.
    - Exemplo: `route print` para exibir a tabela de roteamento.

12. **Pathping**: Combina as funcionalidades do ping e do tracert, fornecendo informações detalhadas sobre cada salto na rota.
    - Exemplo: `pathping <endereço_IP>`

13. **Nslookup**: Utilizado para consultar servidores DNS e resolver nomes de domínio em endereços IP.
    - Exemplo: `nslookup www.exemplo.com`

14. **Wireshark**: Uma poderosa ferramenta de análise de protocolos de rede que permite capturar e examinar o tráfego de rede em tempo real.
    - Exemplo: Capturar pacotes de rede para análise posterior ou em tempo real.


## Situação problema para as ferramentas.

1. **Hostname:**
   - Detalhe: Um usuário relata problemas de conectividade com outros dispositivos na rede.
   - Solução: Use o comando `hostname` para verificar o nome do computador do usuário. Em seguida, verifique se este nome está sendo resolvido corretamente por outros dispositivos na rede usando `nslookup`. Se não estiver, pode ser necessário configurar corretamente o DNS ou o arquivo hosts.

2. **Nbtstat:**
   - Detalhe: Um usuário está tendo problemas para acessar um servidor de arquivos na rede.
   - Solução: Use o comando `nbtstat -a <endereço_IP>` para verificar se o servidor está respondendo e se os nomes NetBIOS estão resolvidos corretamente. Se não estiverem, pode ser necessário verificar as configurações de resolução de nomes na rede ou adicionar manualmente um registro no arquivo LMHOSTS.

3. **Ipconfig:**
   - Detalhe: Um usuário relata que não consegue acessar a Internet.
   - Solução: Use o comando `ipconfig /all` para verificar se o computador recebeu corretamente um endereço IP da rede e se as configurações de DNS estão corretas. Se não estiverem, pode ser necessário corrigir as configurações de rede ou entrar em contato com o administrador de rede.

4. **Getmac:**
   - Detalhe: Um dispositivo desconhecido está aparecendo na rede e você precisa identificar a qual placa de rede ele está associado.
   - Solução: Use o comando `getmac /v` para listar os endereços MAC das placas de rede e suas informações associadas. Compare os endereços MAC listados com o dispositivo desconhecido para identificar a placa de rede associada a ele.

5. **Ping:**
   - Detalhe: Um usuário relata problemas de conectividade com um servidor na rede.
   - Solução: Use o comando `ping <endereço_IP>` para verificar se o servidor está alcançável e se há perda de pacotes. Se houver perda de pacotes, use o `tracert` para identificar onde está ocorrendo o problema na rota até o servidor.

6. **Netstat:**
   - Detalhe: Um servidor está com baixo desempenho e você suspeita que pode estar sendo causado por uma conexão de rede.
   - Solução: Use o comando `netstat -ano` para listar todas as conexões de rede ativas e os processos associados. Identifique quais processos estão consumindo muitos recursos de rede e investigue-os mais a fundo.

7. **Netsh e Arp:**
   - Detalhe: Um dispositivo na rede está sofrendo ataques de ARP spoofing.
   - Solução: Use o comando `arp -a` para verificar se há entradas ARP suspeitas na tabela ARP do dispositivo. Em seguida, use o `netsh interface ip show config` para verificar as configurações de rede do dispositivo e garantir que não haja configurações maliciosas.

8. **Tracert e Route:**
   - Detalhe: Um usuário relata problemas de latência ao acessar um site específico.
   - Solução: Use o comando `tracert <endereço_IP>` para rastrear a rota até o site e identificar possíveis gargalos na rede. Use o `route print` para verificar a tabela de roteamento e garantir que a rota para o site esteja configurada corretamente.

9. **Pathping:**
   - Detalhe: Um usuário relata problemas intermitentes de conectividade com um servidor remoto.
   - Solução: Use o comando `pathping <endereço_IP>` para identificar onde ocorrem os problemas na rota até o servidor. Isso fornecerá informações detalhadas sobre cada salto na rota e ajudará a identificar onde está ocorrendo a perda de pacotes ou aumento da latência.

10. **Nslookup:**
    - Detalhe: Um usuário relata que não consegue acessar um site específico.
    - Solução: Use o comando `nslookup www.exemplo.com` para verificar se o nome de domínio está sendo resolvido corretamente. Se não estiver, verifique as configurações de DNS do dispositivo e do servidor DNS.

11. **Wireshark:**
    - Detalhe: Um administrador de rede precisa investigar um comportamento estranho na rede, como tráfego incomum ou ataques.
    - Solução: Use o Wireshark para capturar e analisar o tráfego de rede em tempo real. Isso permitirá identificar a origem do tráfego incomum e tomar medidas para mitigar qualquer atividade maliciosa.
