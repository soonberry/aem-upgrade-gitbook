# TroubleShooting

1. WCMUse is all replaced by WCMUsePojo from AEM 6.3
2. Adaptive Form button has some change in style tab. The best way for form customize code is to put in a separate tab.
3. Customize Adaptive Form submit type cannot send email out. 
   1. Add `submitService="Email"`  in .content.xml
   2. Give system user `fd-servie ` read permission for customize submit type.
4. jsp-api has been moved from java.servlet to java.servlet.jsp