# JavaMail
Example of Java Mail API with Logs that provides a platform-independent and protocol-independent framework to build mail and messaging applications

Download Java Mail Lib Here : https://maven.java.net/content/repositories/releases/com/sun/mail/javax.mail/1.6.2/javax.mail-1.6.2.jar

package mail; // or <your package name>

```java
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

/**
 *
 * @author ASUS
 */
import javax.mail.*;
import javax.mail.internet.*;
import java.util.*;
import java.io.*;

public class Mail
{
    private static final String SMTP_HOST_NAME = "smtp.gmail.com"; //or simply "localhost"
    private static final String SMTP_HOST_PORT = "587";
    private static final String SMTP_AUTH_USER = "georgienodemon@gmail.com";
    private static final String SMTP_AUTH_PWD  = "fauzan121002";

    public void postMail( String recipients[ ], String subject,
      String message , String from , String contentType , String contentEncoding) throws MessagingException, AuthenticationFailedException
    {
      boolean debug = false;

      //Set the host smtp address
      Properties props = new Properties();
      props.put("mail.smtp.host", SMTP_HOST_NAME);
      props.put("mail.smtp.auth", "true");
      props.put("mail.debug", "true");
      props.put("mail.smtp.port", SMTP_HOST_PORT);
      props.put("mail.smtp.starttls.enable", "true");
      Authenticator auth = new SMTPAuthenticator();
      Session session = Session.getDefaultInstance(props, auth);

      session.setDebug(debug);

      // create a message
      Message msg = new MimeMessage(session);

      // set the from and to address
      InternetAddress addressFrom = new InternetAddress(from);
      msg.setFrom(addressFrom);

      InternetAddress[] addressTo = new InternetAddress[recipients.length];
      for (int i = 0; i < recipients.length; i++)
      {
          addressTo[i] = new InternetAddress(recipients[i]);
      }
      msg.setRecipients(Message.RecipientType.TO, addressTo);

      // Setting the Subject and Content Type
      msg.setHeader("Content-Type",contentType); 
      msg.setHeader("Content-Transfer-Encoding", contentEncoding);
      msg.setSubject(subject);
      msg.setContent(message, contentType);
      
      Transport.send(msg);
      
      Enumeration headers = msg.getAllHeaders();
        while(headers.hasMoreElements()){
            Header header = (Header) headers.nextElement();
            System.out.println("Header: " + header.getName() + ":" + header.getValue());
        }

      System.out.println(msg);
      System.out.println("Successfully sent mail.");
   }

    private class SMTPAuthenticator extends javax.mail.Authenticator
    {
        public PasswordAuthentication getPasswordAuthentication()
        {
            String username = SMTP_AUTH_USER;
            String password = SMTP_AUTH_PWD;
            return new PasswordAuthentication(username, password);
        }
    }
}
```
