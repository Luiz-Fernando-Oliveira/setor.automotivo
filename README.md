# Análise Insumo-Produto do setor automobilístico
Análise do setor automotivo usando o método do Insumo-Produto (Python)

# bibliotecas

import pandas as pd

import numpy as np

# Definindo o caminho

caminho = "C:/Users/lfro9/OneDrive/Documentos/Contas Sociais e Análise Insumo-Produto/"

arquivo = "MIP PASSONI 2000 a 2020.xlsx"

arquivo2 = "tab15_2.xls"

arquivo3 = "Matriz de Agregacao Setorial Emprego.xlsx"

arquivo4 = "MAIs - Ref. 2010.xlsx"

local = caminho + arquivo

local2 = caminho + arquivo2

local3 = caminho + arquivo3

local4 = caminho + arquivo4

# Definindo as coordenadas

Anos = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]

nSetores = 42

lAutomoveis = 29

lPecas = 30

cAutomoveis = 27

cPecas = 28

cVBP = 44

lTotal = 47

cDemandaFinal = 51

lPrimeiroSetor = 4

lUltimoSetor = 45

cPrimeiroSetor = 2

cUltimoSetor = 43

cExportacoes = 45

cConsumoFamilias = 48

cConsumoISFLSF = 47

OrdemSetorAutomoveis = 25

OrdemSetorPecas = 26

# Importando dados de emprego

DadosEmprego = pd.read_excel(local2)

# Agregando setores de DadosEmprego

DadosEmpregoReduzido = DadosEmprego.iloc[5:73,2:13]

MatrizEmprego = np.array(DadosEmpregoReduzido)

MatrizAgregacaoEmprego = pd.read_excel(local3)

MAE = MatrizAgregacaoEmprego.iloc[0:43,1:69]

MAE = MAE.fillna(0)

MAE = np.array(MAE)

MatrizEmpregoAgregada = MAE @ DadosEmpregoReduzido

# Nomeando os dados de emprego

SetoresMIP =  pd.read_excel(local, sheet_name="Recursos 2010").iloc[lPrimeiroSetor:lUltimoSetor+1, 1]

MatrizEmpregoAgregada.index = SetoresMIP 

MatrizEmpregoAgregada.columns = Anos

# Variáveis que não necessitam estar no loop

vZero = 0
    
vZero = np.array(vZero)
    
vZero = vZero.reshape(1, -1)
    
matriz_identidade = np.identity(43)
    
# Calculando os indicadores para cada ano

for ano in Anos:
    # Construir o nome da planilha
    nome_planilha = f"Recursos {ano}"
    
    nome_planilha2 = f"Usos {ano}"
    
    nome_planilha3 = f"Usos Nacional {ano}"
    
    nome_planilha4 = f"Z {ano}"
    
    nome_planilha5 = f"An {ano}"
    
    nome_planilha6 = f"Am {ano}"
    
    nome_planilha7 = f"MAI OT ({ano})"
        
    # Análise do VBP
    vars()[f'V_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha)
    
    vars()[f'VBPAutomóveis_{ano}'] = vars()[f'V_{ano}'].iat[lAutomoveis, cVBP]
    
    vars()[f'VBPPecas_{ano}'] = vars()[f'V_{ano}'].iat[lPecas, cVBP]
    
    vars()[f'VBP_{ano}'] = vars()[f'V_{ano}'].iat[lTotal, cVBP]
    
    vars()[f'vVBP_{ano}'] = vars()[f'V_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1, cVBP]
    
    vars()[f'ParticipacaoAutomóveisVBP_{ano}'] = 100*vars()[f'VBPAutomóveis_{ano}']/vars()[f'VBP_{ano}']
    
    vars()[f'ParticipacaoPecasVBP_{ano}'] = 100*vars()[f'VBPPecas_{ano}']/vars()[f'VBP_{ano}']
    
    # Análise do VA
    
    vars()[f'U_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha2)
    
    vars()[f'CIAutomóveis_{ano}'] = vars()[f'U_{ano}'].iat[lTotal, cAutomoveis]
    
    vars()[f'CIPecas_{ano}'] = vars()[f'U_{ano}'].iat[lTotal, cPecas]
    
    vars()[f'VAAutomoveis_{ano}'] = vars()[f'VBPAutomóveis_{ano}']-vars()[f'CIAutomóveis_{ano}']
    
    vars()[f'VAPecas_{ano}'] = vars()[f'VBPPecas_{ano}']-vars()[f'CIPecas_{ano}']
    
    vars()[f'Un_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha3)
    
    vars()[f'VApb_{ano}'] = vars()[f'Un_{ano}'].iat[lTotal, cDemandaFinal]
    
    vars()[f'ParticipacaoAutomóveisVA_{ano}'] = 100*vars()[f'VAAutomoveis_{ano}']/vars()[f'VApb_{ano}']
    
    vars()[f'ParticipacaoPecasVA_{ano}'] = 100*vars()[f'VAPecas_{ano}']/vars()[f'VApb_{ano}']
    
    # Calculo dos Backward e Foward Linkages diretos e indiretos
    
    vars()[f'L_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha4)
    
    vars()[f'ConsumoDiretoeIndiretoAutomoveis_{ano}'] = vars()[f'L_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1, cAutomoveis]
    
    vars()[f'ConsumoDiretoeIndiretoPecas_{ano}'] = vars()[f'L_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1, cPecas]
    
    vars()[f'BLAutomoveis_{ano}'] = np.sum(vars()[f'ConsumoDiretoeIndiretoAutomoveis_{ano}'], axis=0)
    
    vars()[f'BLPecas_{ano}'] = np.sum(vars()[f'ConsumoDiretoeIndiretoPecas_{ano}'], axis=0)
    
    vars()[f'ValoresL_{ano}'] = vars()[f'L_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1, cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'SomalinhasL_{ano}'] = np.sum(vars()[f'ValoresL_{ano}'], axis=0)
    
    vars()[f'SomaL_{ano}'] = np.sum(vars()[f'SomalinhasL_{ano}'], axis=0)
    
    vars()[f'BLNormalizadoAutomoveis_{ano}'] = nSetores*vars()[f'BLAutomoveis_{ano}']/vars()[f'SomaL_{ano}']
    
    vars()[f'BLNormalizadoPecas_{ano}'] = nSetores*vars()[f'BLPecas_{ano}']/vars()[f'SomaL_{ano}']
    
    vars()[f'OfertaDiretaeIndiretaAutomoveis_{ano}'] = vars()[f'L_{ano}'].iloc[lAutomoveis, cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'OfertaDiretaeIndiretaPecas_{ano}'] = vars()[f'L_{ano}'].iloc[lPecas, cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'FLAutomoveis_{ano}'] = np.sum(vars()[f'OfertaDiretaeIndiretaAutomoveis_{ano}'], axis=0)
    
    vars()[f'FLPecas_{ano}'] = np.sum(vars()[f'OfertaDiretaeIndiretaPecas_{ano}'], axis=0)
    
    vars()[f'FLNormalizadoAutomoveis_{ano}'] = nSetores*vars()[f'FLAutomoveis_{ano}']/vars()[f'SomaL_{ano}']
    
    vars()[f'FLNormalizadoPecas_{ano}'] = nSetores*vars()[f'FLPecas_{ano}']/vars()[f'SomaL_{ano}']
    '''
    # Cálculo do campo de influência
    
    vars()[f'vConsumoDiretoeIndiretoAutomoveis_{ano}'] = np.array(vars()[f'ConsumoDiretoeIndiretoAutomoveis_{ano}'])
    
    vars()[f'vConsumoDiretoeIndiretoPecas_{ano}'] = np.array(vars()[f'ConsumoDiretoeIndiretoPecas_{ano}'])
    
    vars()[f'vOfertaDiretaeIndiretaAutomoveis_{ano}'] = np.array(vars()[f'OfertaDiretaeIndiretaAutomoveis_{ano}'])
    
    vars()[f'vOfertaDiretaeIndiretaPecas_{ano}'] = np.array(vars()[f'OfertaDiretaeIndiretaAutomoveis_{ano}'])
    
    vars()[f'vConsumoDiretoeIndiretoAutomoveis_{ano}'] = vars()[f'vConsumoDiretoeIndiretoAutomoveis_{ano}'].reshape(-1,1)
    
    vars()[f'vConsumoDiretoeIndiretoPecas_{ano}'] = vars()[f'vConsumoDiretoeIndiretoPecas_{ano}'].reshape(-1,1)
    
    vars()[f'vOfertaDiretaeIndiretaAutomoveis_{ano}'] = vars()[f'vOfertaDiretaeIndiretaAutomoveis_{ano}'].reshape(1,-1)
    
    vars()[f'vOfertaDiretaeIndiretaPecas_{ano}'] = vars()[f'vOfertaDiretaeIndiretaPecas_{ano}'].reshape(1,-1)
    
    vars()[f'CampodeInfluenciaAutomoveis_{ano}'] = vars()[f'vConsumoDiretoeIndiretoAutomoveis_{ano}']@vars()[f'vOfertaDiretaeIndiretaAutomoveis_{ano}']
    
    vars()[f'CampodeInfluenciaPecas_{ano}'] = vars()[f'vConsumoDiretoeIndiretoPecas_{ano}']@vars()[f'vOfertaDiretaeIndiretaPecas_{ano}']
    
    vars()[f'SomaLinhasCampoInfluênciaAutomoveis_{ano}'] = np.sum(vars()[f'CampodeInfluenciaAutomoveis_{ano}'], axis=1)
    
    vars()[f'SomaLinhasCampoInfluênciaPecas_{ano}'] = np.sum(vars()[f'CampodeInfluenciaPecas_{ano}'], axis=1)
    
    vars()[f'SomatorioCampoInfluênciaAutomoveis_{ano}'] = np.sum(vars()[f'SomaLinhasCampoInfluênciaAutomoveis_{ano}'], axis=0)
    
    vars()[f'SomatorioCampoInfluênciaPecas_{ano}'] = np.sum(vars()[f'SomaLinhasCampoInfluênciaPecas_{ano}'], axis=0)
    '''
    # Importando os coeficientes diretos nacionais e importados
    
    vars()[f'An_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha5)
    
    vars()[f'Am_{ano}'] = pd.read_excel(local, sheet_name=nome_planilha6)
    
    vars()[f'CoeficientesConsumoNAutomotiveis_{ano}'] = vars()[f'An_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cAutomoveis]
    
    vars()[f'CoeficientesConsumoNPecas_{ano}'] = vars()[f'An_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cPecas]
    
    vars()[f'CoeficientesConsumoMAutomotiveis_{ano}'] = vars()[f'Am_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cAutomoveis]
    
    vars()[f'CoeficientesConsumoMPecas_{ano}'] = vars()[f'Am_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cPecas]
    
    vars()[f'CoeficientesOfertaNAutomotiveis_{ano}'] = vars()[f'An_{ano}'].iloc[lAutomoveis,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'CoeficientesOfertaNPecas_{ano}'] = vars()[f'An_{ano}'].iloc[lPecas,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'CoeficientesOfertaMAutomotiveis_{ano}'] = vars()[f'Am_{ano}'].iloc[lAutomoveis,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'CoeficientesOfertaMPecas_{ano}'] = vars()[f'Am_{ano}'].iloc[lPecas,cPrimeiroSetor:cUltimoSetor+1]
    
    # Detalhando os Coeficientes (ofertante x demandante) Diretos Nacionis (DN) e Diretos Importados (DM)
    
    vars()[f'CoeficienteDN_AxA_{ano}'] = vars()[f'An_{ano}'].iloc[lAutomoveis,cAutomoveis]
    
    vars()[f'CoeficienteDN_PxP_{ano}'] = vars()[f'An_{ano}'].iloc[lPecas,cPecas]
    
    vars()[f'CoeficienteDN_AxP_{ano}'] = vars()[f'An_{ano}'].iloc[lAutomoveis,cPecas]
    
    vars()[f'CoeficienteDN_PxA_{ano}'] = vars()[f'An_{ano}'].iloc[lPecas, cAutomoveis]
    
    vars()[f'CoeficienteDM_AxA_{ano}'] = vars()[f'Am_{ano}'].iloc[lAutomoveis,cAutomoveis]
    
    vars()[f'CoeficienteDM_PxP_{ano}'] = vars()[f'Am_{ano}'].iloc[lPecas,cPecas]
    
    vars()[f'CoeficienteDM_AxP_{ano}'] = vars()[f'Am_{ano}'].iloc[lAutomoveis,cPecas]
    
    vars()[f'CoeficienteDM_PxA_{ano}'] = vars()[f'Am_{ano}'].iloc[lPecas, cAutomoveis]
    
    # Cálculo dos Backward e Foward Linkages diretos nacionais (BLDN) e importados (BLDM)
    
    vars()[f'BLDN_Automoveis_{ano}'] = np.sum(vars()[f'CoeficientesConsumoNAutomotiveis_{ano}'], axis=0)
    
    vars()[f'BLDN_Pecas_{ano}'] = np.sum(vars()[f'CoeficientesConsumoNPecas_{ano}'], axis=0)
    
    vars()[f'BLDM_Automoveis_{ano}'] = np.sum(vars()[f'CoeficientesConsumoMAutomotiveis_{ano}'], axis=0)
    
    vars()[f'BLDM_Pecas_{ano}'] = np.sum(vars()[f'CoeficientesConsumoMPecas_{ano}'], axis=0)
    '''
    vars()[f'FLDN_Automoveis_{ano}'] = np.sum(vars()[f'CoeficientesOfertaNAutomotiveis_{ano}'], axis=0)
    
    vars()[f'FLDN_Pecas_{ano}'] = np.sum(vars()[f'CoeficientesOfertaNPecas_{ano}'], axis=0)
    
    vars()[f'FLDM_Automoveis_{ano}'] = np.sum(vars()[f'CoeficientesOfertaMAutomotiveis_{ano}'], axis=0)
    
    vars()[f'FLDM_Pecas_{ano}'] = np.sum(vars()[f'CoeficientesOfertaMPecas_{ano}'], axis=0)
    '''
    # Cálculo da Participação dos coeficientes técnicos nacionais no total do CTs
    
    ## Totais
    vars()[f'BLDN+M_Automoveis_{ano}'] = vars()[f'BLDN_Automoveis_{ano}']+vars()[f'BLDM_Automoveis_{ano}'] 
    
    vars()[f'BLDN+M_Pecas_{ano}'] = vars()[f'BLDN_Pecas_{ano}']+vars()[f'BLDM_Pecas_{ano}']
    
    vars()[f'ParticipacaoCTNacional_Automoveis_{ano}'] = vars()[f'BLDN_Automoveis_{ano}']/vars()[f'BLDN+M_Automoveis_{ano}']
    
    vars()[f'ParticipacaoCTNacional_Pecas_{ano}'] = vars()[f'BLDN_Pecas_{ano}']/vars()[f'BLDN+M_Pecas_{ano}']
    
    ## Dos setores selecionados
    
    vars()[f'Coeficientes_N+M_AxA_{ano}'] = vars()[f'CoeficienteDN_AxA_{ano}']+vars()[f'CoeficienteDM_AxA_{ano}']
    
    vars()[f'Coeficientes_N+M_PxA_{ano}'] = vars()[f'CoeficienteDN_PxA_{ano}']+vars()[f'CoeficienteDM_PxA_{ano}']
    
    vars()[f'Coeficientes_N+M_AxP_{ano}'] = vars()[f'CoeficienteDN_AxP_{ano}']+vars()[f'CoeficienteDM_AxP_{ano}']
    
    vars()[f'Coeficientes_N+M_PxP_{ano}'] = vars()[f'CoeficienteDN_PxP_{ano}']+vars()[f'CoeficienteDM_PxP_{ano}']
    
    vars()[f'ParticipacaoAxAnacional_{ano}'] = vars()[f'CoeficienteDN_AxA_{ano}']/vars()[f'Coeficientes_N+M_AxA_{ano}']
    
    vars()[f'ParticipacaoPxPnacional_{ano}'] = vars()[f'CoeficienteDN_PxP_{ano}']/vars()[f'Coeficientes_N+M_PxP_{ano}']
    
    vars()[f'ParticipacaoAxPnacional_{ano}'] = vars()[f'CoeficienteDN_AxP_{ano}']/vars()[f'Coeficientes_N+M_AxP_{ano}']
    
    vars()[f'ParticipacaoPxAnacional_{ano}'] = vars()[f'CoeficienteDN_PxA_{ano}']/vars()[f'Coeficientes_N+M_PxA_{ano}']
    
    # exportações, importações, exportações líquidas, participação das exportações na DF
    
    vars()[f'ExportacoesAutomoveis_{ano}'] = vars()[f'Un_{ano}'].iloc[lAutomoveis,cExportacoes]
    
    vars()[f'ExportacoesPecas_{ano}'] = vars()[f'Un_{ano}'].iloc[lPecas,cExportacoes]
    
    vars()[f'ImportacoesAutomoveis_{ano}'] = vars()[f'BLDM_Automoveis_{ano}']*vars()[f'VBPAutomóveis_{ano}']
    
    vars()[f'ImportacoesPecas_{ano}'] = vars()[f'BLDM_Pecas_{ano}']*vars()[f'VBPPecas_{ano}']
    
    vars()[f'ExportacoesLiquidasAutomoveis_{ano}'] = vars()[f'ExportacoesAutomoveis_{ano}'] - vars()[f'ImportacoesAutomoveis_{ano}']
    
    vars()[f'ExportacoesLiquidasPecas_{ano}'] = vars()[f'ExportacoesPecas_{ano}'] - vars()[f'ImportacoesPecas_{ano}'] 
    
    vars()[f'ParticipacaoExportacoesDFAutomoveis_{ano}'] = vars()[f'ExportacoesAutomoveis_{ano}']/vars()[f'Un_{ano}'].iloc[lAutomoveis,cDemandaFinal]
    
    vars()[f'ParticipacaoExportacoesDFPecas_{ano}'] = vars()[f'ExportacoesPecas_{ano}']/vars()[f'Un_{ano}'].iloc[lPecas,cDemandaFinal]
    
    # Emprego e coeficiente de emprego (Tabelas Sinóticas)
    
    vars()[f'Empregos_{ano}'] = MatrizEmpregoAgregada.loc[:, ano]
    
    vars()[f'Empregos_{ano}'].index = SetoresMIP
    
    vars()[f'Empregos_Automoveis_{ano}'] = vars()[f'Empregos_{ano}'].loc["Automóveis camionetas caminhões e ônibus"]
    
    vars()[f'Empregos_Pecas_{ano}'] = vars()[f'Empregos_{ano}'].loc["Peças e acessórios para veículos automotores"]
    
    vars()[f'Coeficiente_Empregos_Automoveis_{ano}'] = vars()[f'Empregos_Automoveis_{ano}']/vars()[f'VBPAutomóveis_{ano}']
    
    vars()[f'Coeficiente_Empregos_Pecas_{ano}'] = vars()[f'Empregos_Pecas_{ano}']/vars()[f'VBPPecas_{ano}']
    
    vars()[f'Relacao_PIBEmprego_Automoveis_{ano}'] = 1000000*vars()[f'VAAutomoveis_{ano}']/vars()[f'Empregos_Automoveis_{ano}']
    
    vars()[f'Relacao_PIBEmprego_Pecas_{ano}'] = 1000000*vars()[f'VAPecas_{ano}']/vars()[f'Empregos_Pecas_{ano}']
    
    vars()[f'vVBP_{ano}'].index = SetoresMIP
    
    vars()[f'vCoeficienteEmprego_{ano}'] = vars()[f'Empregos_{ano}']/vars()[f'vVBP_{ano}']
    
    # Investimento e coeficiente de investimento (MAI)
    
    vars()[f'MAI_OT_{ano}'] = pd.read_excel(local4, sheet_name=nome_planilha7)
    
    vars()[f'MAI_OT_{ano}'].set_axis(np.arange(len(vars()[f'MAI_OT_{ano}'].columns)), axis=1, inplace=True)
    
    vars()[f'FBCF_Automoveis_{ano}'] = vars()[f'MAI_OT_{ano}'].iloc[27,32]
    
    vars()[f'FBCF_Pecas_{ano}'] = vars()[f'MAI_OT_{ano}'].iloc[27,33]
    
    vars()[f'Coeficiente_FBCF_Automoveis_{ano}'] = vars()[f'FBCF_Automoveis_{ano}']/vars()[f'VBPAutomóveis_{ano}']
    
    vars()[f'Coeficiente_FBCF_Pecas_{ano}'] = vars()[f'FBCF_Pecas_{ano}']/vars()[f'VBPPecas_{ano}']
    
    # Endogenizando renda e consumo
    
    vars()[f'vCI_{ano}'] = vars()[f'U_{ano}'].iloc[lTotal,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'vCI_{ano}'] = vars()[f'vCI_{ano}'].reset_index(drop=True)
    
    vars()[f'vVBP_{ano}'] = vars()[f'vVBP_{ano}'].reset_index(drop=True)
    
    vars()[f'vVA_{ano}'] = vars()[f'vVBP_{ano}']-vars()[f'vCI_{ano}']
    
    vars()[f'vCoeficienteVA_{ano}'] = vars()[f'vVA_{ano}']/vars()[f'vVBP_{ano}']
    
    vars()[f'vConsumoFamilias_{ano}'] = vars()[f'Un_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cConsumoFamilias]
    
    vars()[f'TotalConsumoFamilias_{ano}'] = vars()[f'Un_{ano}'].iloc[lTotal,cConsumoFamilias]
    
    vars()[f'vConsumoISFLSF_{ano}'] = vars()[f'Un_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cConsumoISFLSF]
    
    vars()[f'TotalConsumoISFLSF_{ano}'] = vars()[f'Un_{ano}'].iloc[lTotal,cConsumoISFLSF]
    
    vars()[f'vConsumoFamiliasISFLSF_{ano}'] = vars()[f'vConsumoFamilias_{ano}']+vars()[f'vConsumoISFLSF_{ano}']
    
    vars()[f'TotalConsumoFamiliasISFLSF_{ano}'] = vars()[f'TotalConsumoFamilias_{ano}']+vars()[f'TotalConsumoISFLSF_{ano}']
    
    vars()[f'vCoeficienteConsumoFamilias_{ano}'] = vars()[f'vConsumoFamiliasISFLSF_{ano}']/vars()[f'TotalConsumoFamiliasISFLSF_{ano}']
    
    vars()[f'vCoeficienteVA_{ano}'] = np.array(vars()[f'vCoeficienteVA_{ano}'])
    
    vars()[f'vCoeficienteConsumoFamilias_{ano}'] = np.array(vars()[f'vCoeficienteConsumoFamilias_{ano}'])
    
    vars()[f'vCoeficienteVA_{ano}'] = vars()[f'vCoeficienteVA_{ano}'].reshape(1,-1)
    
    vars()[f'vCoeficienteConsumoFamilias_{ano}'] = vars()[f'vCoeficienteConsumoFamilias_{ano}'].reshape(-1,1)
    
    vars()[f'vCoeficienteVA_{ano}'] = np.concatenate((vars()[f'vCoeficienteVA_{ano}'],vZero), axis=1)
    
    vars()[f'PropensaodeConsumiraRenda_{ano}'] = vars()[f'TotalConsumoFamiliasISFLSF_{ano}']/vars()[f'VApb_{ano}']
    
    vars()[f'vCoeficienteRendaConsumida_{ano}'] = vars()[f'PropensaodeConsumiraRenda_{ano}']*vars()[f'vCoeficienteVA_{ano}']
    
    vars()[f'MatrizA_{ano}'] = vars()[f'An_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'MatrizA_modificada_{ano}'] = np.append(vars()[f'MatrizA_{ano}'],vars()[f'vCoeficienteConsumoFamilias_{ano}'], axis=1)
    
    vars()[f'MatrizA_modificada_{ano}'] = np.append(vars()[f'MatrizA_modificada_{ano}'],vars()[f'vCoeficienteRendaConsumida_{ano}'],axis=0)
    
    vars()[f'matriz_auxiliar_{ano}'] = matriz_identidade - vars()[f'MatrizA_modificada_{ano}']
    
    vars()[f'matriz_auxiliar_{ano}'] = vars()[f'matriz_auxiliar_{ano}'].astype(float)
    
    vars()[f'mLmodificada_{ano}'] = np.linalg.inv(vars()[f'matriz_auxiliar_{ano}'])
    
    # Multiplicador de empgrego e produção tipo 1 e tipo 2
    
    ## Produção
    
    vars()[f'MultiplicadorProducaoTipoII_{ano}'] = np.sum(vars()[f'mLmodificada_{ano}'], axis=0)
    
    vars()[f'MPTotal_Automoveis_{ano}'] = vars()[f'MultiplicadorProducaoTipoII_{ano}'][OrdemSetorAutomoveis,]
    
    vars()[f'MPTotal_Pecas_{ano}'] = vars()[f'MultiplicadorProducaoTipoII_{ano}'][OrdemSetorPecas,]
    
    vars()[f'MPInduzido_Automoveis_{ano}'] = vars()[f'MPTotal_Automoveis_{ano}'] - vars()[f'BLAutomoveis_{ano}']
    
    vars()[f'MPInduzido_Pecas_{ano}'] = vars()[f'MPTotal_Pecas_{ano}'] - vars()[f'BLPecas_{ano}']
    
    vars()[f'MPIndireto_Automoveis_{ano}'] = vars()[f'MPInduzido_Automoveis_{ano}'] - vars()[f'BLDN_Automoveis_{ano}']
    
    vars()[f'MPIndireto_Pecas_{ano}'] = vars()[f'MPInduzido_Pecas_{ano}'] - vars()[f'BLDN_Pecas_{ano}']
    
    ## Emprego
    
    vars()[f'DiagRequisitosEmprego_{ano}'] = np.diag(vars()[f'vCoeficienteEmprego_{ano}'])
    
    vars()[f'mL_{ano}'] = vars()[f'L_{ano}'].iloc[lPrimeiroSetor:lUltimoSetor+1,cPrimeiroSetor:cUltimoSetor+1]
    
    vars()[f'mL_{ano}'] = np.array(vars()[f'mL_{ano}'])
    
    vars()[f'mGeradorEmprego_{ano}'] = vars()[f'DiagRequisitosEmprego_{ano}'] @ vars()[f'mL_{ano}']
    
    vars()[f'MultEmpregoSimples_{ano}'] = np.sum(vars()[f'mGeradorEmprego_{ano}'], axis=0)
    
    vars()[f'MultEmpregoI_{ano}'] = vars()[f'MultEmpregoSimples_{ano}']/vars()[f'vCoeficienteEmprego_{ano}']
    
    vars()[f'MultEmpregoSimplesAutomoveis_{ano}'] = vars()[f'MultEmpregoI_{ano}'][OrdemSetorAutomoveis]
    
    vars()[f'MultEmpregoSimplesPecas_{ano}'] = vars()[f'MultEmpregoI_{ano}'][OrdemSetorPecas]
    
    vars()[f'mGeradorEmpregoII_{ano}'] = vars()[f'DiagRequisitosEmprego_{ano}'] @ vars()[f'mLmodificada_{ano}'][:-1,:-1]
    
    vars()[f'MultEmpregoTotal_{ano}'] = np.sum(vars()[f'mGeradorEmpregoII_{ano}'], axis=0)
    
    vars()[f'MultEmpregoII_{ano}'] = vars()[f'MultEmpregoTotal_{ano}']/vars()[f'vCoeficienteEmprego_{ano}']
    
    vars()[f'MultEmpregoTotalAutomoveis_{ano}'] = vars()[f'MultEmpregoII_{ano}'][OrdemSetorAutomoveis]
    
    vars()[f'MultEmpregoTotalPecas_{ano}'] = vars()[f'MultEmpregoII_{ano}'][OrdemSetorPecas]
    
    vars()[f'EfeitoInduzidoEmpregoAutomoveis_{ano}'] = vars()[f'MultEmpregoTotalAutomoveis_{ano}'] - vars()[f'MultEmpregoSimplesAutomoveis_{ano}']
    
    vars()[f'EfeitoInduzidoEmpregoPecas_{ano}'] = vars()[f'MultEmpregoTotalPecas_{ano}'] - vars()[f'MultEmpregoSimplesPecas_{ano}']
    
    vars()[f'EfeitoDiretoEmpregoAutomoveis_{ano}'] = vars()[f'Coeficiente_Empregos_Automoveis_{ano}']
    
    vars()[f'EfeitoDiretoEmpregoPecas_{ano}'] = vars()[f'Coeficiente_Empregos_Pecas_{ano}']
    
    vars()[f'EfeitoIndiretoEmpregoAutomoveis_{ano}'] = vars()[f'MultEmpregoSimplesAutomoveis_{ano}'] - vars()[f'EfeitoDiretoEmpregoAutomoveis_{ano}']
    
    vars()[f'EfeitoIndiretoEmpregoPecas_{ano}'] = vars()[f'MultEmpregoSimplesPecas_{ano}'] - vars()[f'EfeitoDiretoEmpregoPecas_{ano}']
    
# Construindo Tabela com os principais indicadores

nomes_variaveis = [
    'VBPAutomóveis', 'VBPPecas', 'ParticipacaoAutomóveisVBP', 'ParticipacaoPecasVBP',
    'VAAutomoveis', 'VAPecas', 'ParticipacaoAutomóveisVA', 'ParticipacaoPecasVA',
    'BLAutomoveis', 'BLPecas', 'BLNormalizadoAutomoveis', 'BLNormalizadoPecas',
    'FLAutomoveis', 'FLPecas', 'FLNormalizadoAutomoveis', 'FLNormalizadoPecas',
    'CoeficienteDN_AxA', 'CoeficienteDN_PxP', 'CoeficienteDN_AxP', 'CoeficienteDN_PxA','ParticipacaoCTNacional_Automoveis',
    'ParticipacaoCTNacional_Pecas', 'ParticipacaoAxAnacional', 'ParticipacaoPxPnacional',
    'ParticipacaoAxPnacional', 'ParticipacaoPxAnacional', 'ExportacoesAutomoveis',
    'ExportacoesPecas', 'ImportacoesAutomoveis', 'ImportacoesPecas', 'ExportacoesLiquidasAutomoveis',
    'ExportacoesLiquidasPecas', 'ParticipacaoExportacoesDFAutomoveis', 'ParticipacaoExportacoesDFPecas',
    'Empregos_Automoveis', 'Empregos_Pecas', 'FBCF_Automoveis', 'FBCF_Pecas',
    'Coeficiente_FBCF_Automoveis', 'Coeficiente_FBCF_Pecas', 'MPTotal_Automoveis', 'MPTotal_Pecas',
    'MPInduzido_Automoveis', 'MPInduzido_Pecas', 'MPIndireto_Automoveis', 'MPIndireto_Pecas',
    'MultEmpregoTotalAutomoveis', 'MultEmpregoTotalPecas', 'EfeitoInduzidoEmpregoAutomoveis',
    'EfeitoInduzidoEmpregoPecas','EfeitoDiretoEmpregoAutomoveis','EfeitoDiretoEmpregoPecas',
    'EfeitoIndiretoEmpregoAutomoveis','EfeitoIndiretoEmpregoPecas',
    'Relacao_PIBEmprego_Automoveis','Relacao_PIBEmprego_Pecas'
]

# Dicionário para armazenar os dados
data = {}

# Preenchendo o dicionário com os valores das variáveis para cada ano
for nome_variavel in nomes_variaveis:
    # Acessando cada ano e obtenha o valor da variável correspondente
    valores = [eval(f'{nome_variavel}_{ano}') for ano in Anos]

    # Adicione a lista de valores ao dicionário
    data[nome_variavel] = valores
    

# Criando o DataFrame
df = pd.DataFrame(data, index=Anos)

# Salvando o DataFrame como excel

Arquivo5 = "Indicadores MIP Automobilistico 2010 2020.xlsx"

local5 = caminho + Arquivo5

df.to_excel(local5)

# Gerando uma tabela com dados de emprego

NomesSetores = ["Automoveis", "Pecas"]

QuadroEmpregos = pd.DataFrame(index = NomesSetores, columns = Anos)

for setor in NomesSetores:
    
    for ano in Anos:
    
        dados_emprego = f'Empregos_{setor}_{ano}'
        
        valor = globals()[dados_emprego]
        
        QuadroEmpregos.at[setor, ano] = valor
        
QuadroEmpregos = QuadroEmpregos.transpose()

Arquivo6 = "Dados Emprego Tabelas Sinoticas 2010 2022.xlsx"

local6 = caminho+Arquivo6

QuadroEmpregos.to_excel(local6)
