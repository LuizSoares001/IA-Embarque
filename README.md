# IA-Embarque

Esse é um projeto de inteligência artificial voltado para a previsão de falhas em máquinas industriais. O modelo é treinado com um dataset contendo 10.000 amostras, onde são aplicadas técnicas de tratamento e balanceamento de dados, uma vez que o número de máquinas com falhas é significativamente menor do que aquelas em bom funcionamento.

Inicialmente, o dataset apresenta cinco tipos de falhas: TWF, HDF, PWF, OSF e RNF. No entanto, as falhas do tipo RNF são removidas, por se tratarem de falhas aleatórias — o que inviabiliza a criação de um padrão confiável para previsão.

Um dos principais desafios do projeto é a baixa quantidade de amostras da falha TWF, mesmo após a exclusão das falhas aleatórias. Para lidar com esse desbalanceamento, são aplicadas técnicas como undersampling e class weighting durante o treinamento.

Após a finalização e validação do modelo, a proposta é embarcá-lo em uma placa STM32, concluindo o ciclo completo do projeto, da construção até a aplicação em hardware embarcado.
