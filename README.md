Desafio de Projeto: Implementando sua Primeira Stack com AWS CloudFormation

Descrição do Projeto e Arquitetura

O projeto consistiu na criação e execução de um template declarativo estruturado em **YAML**. A stack provisionada simula um ambiente real de servidor web composto pelos seguintes recursos estruturais:

1.Instância EC2 (AWS::EC2::Instance): Um servidor virtual configurado com o tipo de instância `t2.micro` rodando a imagem Amazon Linux 2 (atualizada dinamicamente via Parameter Store).
2.Automação via UserData: Inclusão de um script Bash inline executado automaticamente no boot da máquina para atualização dos pacotes do sistema, instalação, inicialização e ativação do servidor web Apache (httpd), gerando uma página de boas-vindas dinâmica.
3. Grupo de Segurança (AWS::EC2::SecurityGroup): Definição de regras de firewall a nível de instância (Ingress rules) para permitir o tráfego essencial:
Porta 80 (HTTP): Aberta publicamente para que utilizadores externos acedam à aplicação web.
Porta 22 (SSH): Aberta para administração remota segura baseada no IP de origem.

Tecnologias e Conceitos Utilizados

- AWS CloudFormation: Serviço de modelagem e provisionamento de recursos de infraestrutura.
- Amazon EC2 (Elastic Compute Cloud): Servidores virtuais escaláveis na nuvem.
- Security Groups: Firewalls virtuais para controlo de tráfego de entrada e saída.
- YAML (Yet Another Markup Language): Linguagem de serialização de dados utilizada para escrever o template IaC.
- Git & GitHub: Versionamento de código e documentação técnica do ecossistema.

Como Executar este Projeto na AWS

Para replicar esta infraestrutura na sua conta AWS, siga os passos abaixo:

1. Aceder ao Console: Faça login na Console de Gerenciamento da AWS e selecione a região pretendida (ex: us-east-1).
2. Aceder ao CloudFormation: Na barra de pesquisa, procure por CloudFormation.
3. Criar uma Nova Pilha (Stack):
   - Clique em Create stack > *With new resources (standard).
   - Em Prepare template, selecione **Template is ready.
   - Em Specify template, escolha Upload a template file e envie o arquivo contido na pasta deste repositório.
4. Configurar Parâmetros:
   - Dê um nome à pilha (ex: MyTestStack).
   - Ajuste o parâmetro MyIP para especificar a gama de IPs permitida ou utilize 0.0.0.0/0 para testes públicos abertos.
5. Conclusão: Avance pelas opções mantendo as configurações padrão e clique em Submit. Acompanhe a aba *Events* até que o status mude para CREATE_COMPLETE.

 Insights e Aprendizados Adquiridos

- Poder da Automação (IaC): A capacidade de instanciar servidores e configurar firewalls complexos em segundos através de um simples arquivo de texto reduz drasticamente o erro humano e o tempo operacional comparado ao processo manual via console visual.
- Gestão de Rollbacks: Durante o desenvolvimento, o CloudFormation demonstrou a sua robustez ao executar o processo de Rollback automático quando uma falha foi detectada (como parâmetros de rede ou IPs inválidos como 0.0.0.0/32), garantindo que recursos parcialmente criados e com configurações inconsistentes não gerassem custos ou falhas de segurança órfãs na conta.
- Modularidade e Reutilização: A utilização de blocos de Parameters e referências dinâmicas (!Ref, !Sub, !GetAtt) provou ser indispensável para criar templates genéricos que podem ser reaproveitados em diferentes ambientes (Desenvolvimento, Homologação e Produção) apenas alterando os inputs iniciais.
