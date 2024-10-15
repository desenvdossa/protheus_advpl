/*
Enviar email via workflow ao responsável quando houver uma nova admissão
*/


User Function GP010AGRV() //Função ativada pelo ponto de entrada do cadastro de funcionários

Local cAssunto 
Local cDe      
Local cPara
Local cTexto      
Local cQuery
local nOpc



IF TYPE ("PARAMIXB") == "A"

	nOpc = PARAMIXB[1]

else 

	nOpc = 3

end if

If(nOpc == 3)



	If Select("SRA") = 0
		lJob := .T.
		RPCSetEnv("05", "01", "", "", "", "", {"SRV"})
	end if

  	cQuery := "	SELECT T1.RA_MAT, T1.RA_NOME, T1.RA_OBSERVA, CARGOS.Q3_DRELINT  FROM " + RetSqlName("SRA") + " T1 
  	cQuery += "	LEFT JOIN (SELECT Q3_DRELINT, Q3_CARGO
  	cQuery += "				      FROM SQ3010 
  	cQuery += "  )CARGOS ON T1.RA_CARGO = CARGOS.Q3_CARGO 
  	cQuery += " WHERE T1.D_E_L_E_T_ = '' AND T1.RA_MAT ='" + SRA->RA_MAT + "' "
  	TcQuery cQuery new alias 'LIN'  

  	DbSelectarea('LIN')
  	DbGoTop()
	

  	cPara := LIN->RA_OBSERVA
  	cTexto   := "<BODY><HTML>Treinamentos necessários para "+ LIN->RA_NOME + " "
  	cTexto   += "<br><br>"

  	cQuery 	:= "	SELECT  RDY_CHAVE, RDY_TEXTO, RDY_CAMPO FROM RDY050 WHERE RDY_CHAVE = '" + LIN->Q3_DRELINT + "' "
  	TcQuery cQuery new alias 'LIL'


  	DbSelectarea('LIL')   
  	DbGoTop()  


    	While LIL->(!EOF())  
				cTexto += StrTran(LIL->RDY_TEXTO, "\13\10", "")
				cTexto += StrTran(LIL->RDY_TEXTO, "\14\10", "")
			 	DbSelectarea('LIL')
			 	DbSkip()
		ENDDO			

	cTexto   += "<br><br>Este e-mail é de carater informativo, para que sejam tomadas as devidas providências em relação os treinamentos do recém-admitido e/ou promovido."
	cTexto   += "<br>Por favor não responda a este e-mail!"

	cAssunto := "Treinamentos - Novo colaborador" 
	cDe := 'workflow'
    U_EnviarEMail(cDe, cPara, cAssunto, cTexto+"</BODY></HTML>")                   
    LIL->(DbCloseArea())
  	LIN->(DbCloseArea())

end if

Return 
