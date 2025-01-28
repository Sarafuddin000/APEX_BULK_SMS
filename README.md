# APEX_BULK_SMS
Sending Bulk SMS with Oracle Apex (http://apex.oracle.com) without SSL/Wallet Configuration.


    DECLARE
        L_CLOB       CLOB;
        V_MSG        VARCHAR2 (4000);
        P_REICEVER   VARCHAR (4000) := 'Your Comma seperated Numbers Goes here';
        P_MESSAGE    VARCHAR2 (4000) := 'Your Message Goes Here';
        P_TOKEN      VARCHAR2 (50) := 'Your Token Goes Here';
    BEGIN
            SELECT    '%'
                   || LISTAGG (SUBSTR (RAWTOHEX (P_MESSAGE), LEVEL * 2 - 1, 2), '%')
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
                       'http://api.greenweb.com.bd/api.php?token='
                    || p_token
                    || ''
                    || CHR (38)
                    || 'to='
                    || P_REICEVER
                    || ''
                    || CHR (38)
                    || 'message='
                    || V_MSG
                    || '',
                P_HTTP_METHOD   => 'GET');
                
        --        DBMS_OUTPUT.put_line (
        --               'http://api.greenweb.com.bd/api.php?token='||p_token||''
        --            || CHR (38)
        --            || 'to='
        --            || P_REICEVER
        --            || ''
        --            || CHR (38)
        --            || 'message='
        --            || V_MSG
        --            || '');
    
    END;
