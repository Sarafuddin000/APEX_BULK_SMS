# APEX_BULK_SMS
Sending Bulk SMS with Oracle Apex (http://apex.oracle.com) without SSL/Wallet Configuration. 
Oracle Apex we can't access HTTPS protocols without having SSL / Wallets. But if API supports HTTP Request then we can send Bulk SMS using BDBULKSMS.NET / GREENWEB.COM.BD. 

Reciever:    may have a single number or comma separated multiple number, or you can use loops.

Message:     You can use Engilsh / Bengali Unicode to write SMSs

Token:       API Provider will provide unique token, that need to use here.



    DECLARE
        L_CLOB       CLOB;
        V_MSG        VARCHAR2 (4000);
        P_REICEVER   VARCHAR (4000) := 'Your Comma seperated Numbers Goes here';
        P_MESSAGE    VARCHAR2 (2000) := 'Your Message Goes Here';
        P_TOKEN      VARCHAR2 (50) := 'Your Token Goes Here';
    BEGIN
            SELECT '%'|| LISTAGG (SUBSTR (RAWTOHEX (P_MESSAGE), LEVEL * 2 - 1, 2), '%')
                          WITHIN GROUP (ORDER BY LEVEL)    "URI_ENCODED"
              INTO V_MSG
              FROM DUAL
        CONNECT BY LEVEL <= LENGTH (RAWTOHEX (P_MESSAGE)) / 2;
    
      SELECT LISTAGG (mobile, ',') WITHIN GROUP (ORDER BY mobile)
          INTO P_REICEVER
          FROM Your_table;
    
       L_CLOB :=
            APEX_WEB_SERVICE.MAKE_REST_REQUEST (
                P_URL           =>
                       'http://api.greenweb.com.bd/api.php?token='|| p_token|| ''
                    || CHR (38)|| 'to='|| P_REICEVER|| ''
                    || CHR (38)|| 'message='|| V_MSG|| '',
                P_HTTP_METHOD   => 'GET');
    END;

Put this code into dynamic action / after submit process. 
