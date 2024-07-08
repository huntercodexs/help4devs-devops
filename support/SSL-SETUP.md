
# SSL SETUP

###### KEYSTORE / JKS
	
- CREATE A ENTITY

<pre>
- Open the KeyStore Explorer
- Click in New (New KeyStore Type)
- Select the JKS option and click in OK button
- Get the PCK#12 (.p12) file and put it in the folder
- In the KeyStore Explorer click in Tools menu > Import Key Pair Type
- Select PKCS#12 and click in OK
- In the next step put the password in the input text and choose the PKCS #12 file
- Finally click in Import
- Create any alias that you want (example: tomcat) or let as is
- Inform the password in the next screen
- You will see the successful message "Key Pair Import Successful"
- Click in OK
</pre>

- CREATE A CERTIFICATE TO ENTITY - IMPORTANT

<pre>
- Get the certificate CRT from godaddy and put it in the same folder, example: e4e6909341907c94.crt
- Give a double click in the CRT certificate > Certification Path > Double Click in the First (Root) certificate
- Go to Details Tab > Copy to File
- Follow the steps and give a name root-cert.cer for it
- Now give a double Click in the Second (Intermediate) certificate in the same certificate window
- Goto Details Tab > Copy to File
- Follow the steps and give a name intermediate-cert.cer for it
</pre>

- IMPORT THE CERTIFICATES CREATED

<pre>
- Now in the KeyStore Explorer click in Tools > Import Trusted Certificate
- Select the root-cert.cer and let alias as is (just click in OK)
- Do the same thing for the intermediate-cert.cer
- Click right button in the tomcat entity > Edit Certificate Chain > Remove Certificate (dot it two times)
- Click right button in the tomcat entity > Import CA reply > From file > [Select PEM File]
- Click right button in the tomcat entity > Edit Certificate Chain > Append Certificate (choose intermediate-cert.cer)
- Click right button in the tomcat entity > Edit Certificate Chain > Append Certificate (choose root-cert.cer)
- Click right button in the tomcat entity > Verify Certificate > Click in OK button (receive a success response)
- Now the keystore is ready to save
- Click in File menu > Save
- Use the keystore into HTTPS Service required
</pre>

> NOTE: If there is any keystore generated before, use it !

###### CERT / NGINX

- Step by step

<pre>
- Download the certificate file from your provider
    Godday Example:
    My Products > SSL Certificate > [Manager] > "Download Certificate File"> Choose Certificate Type[Apache, Tomcat]
</pre>

<pre>
- unzip file.zip
</pre>

<pre>
- Create the main certificate domain 
    Example:
    mv 27dfb4efe033e4c2.crt ${domain}.crt
</pre>

<pre>
- Create the intermediate certificate
    Example:
    mv gd_bundle-g2-g1.crt intermediate.crt
</pre>

<pre>
- Join the certificate file into a single file named *.chained.crt
    Example:
    cat ${domain}.crt intermediate.crt > ${domain}.chained.crt
</pre>

<pre>
- Update the NGINX configurations
    sudo su
    cd /etc/nginx/ssl/
    mv ${domain}.chained.crt ${domain}.chained_${date}.crt
    cp ${path}/${domain}.chained.crt .
</pre>

<pre>
- Restart the NGINX service
    sudo systemctl restart nginx
    sudo systemctl status nginx
</pre>

<pre>
- Test
    https://${domain}
    Check the certificate due date
</pre>