# Apache-Solr-Installation-Execution
Apache solr in windows and linux server

# Solr Download and Installation :
Step - 1: Collect the Solr TAR Archive file (https://lucene.apache.org/solr/downloads.html). Now you have to extract that archive file at target directory of server.

Step – 2: To extract solr, execute the bellow command 
       tar zxf solr-5.3.1.tgz -C targetDirectory
targetDirectory (Ex : /apps/solr)  is the directory where solr will be installed. Solr runs of Java 7 or          greater. Before we start the solr server, validate JAVA_HOME is set on machine.

Step - 3: Go to the solr directory (Ex: /apps/solr/solr-5.3.1) and execute bellow command to start solr.
       bin/solr start
solr will start at default port 8983. For different port, use the bellow command.
       bin/solr start -p 8984
It will start solr at 8984 port. The output will be like this
        Waiting up to 30 seconds to see Solr running on port 8984 [\]
       Started Solr server on port 8984 (pid=22781). Happy searching!

Step - 4: Go to the bellow directory for checking solr.
      http://localhost:8984/solr/ like http://HOST:PORT/solr/
      
# Solr core creation : 
Go to the solr directory (Ex: /apps/solr/solr-5.3.1).
For core creation, execute the below command.
bin/solr create -c CORE_NAME -p 8983 -d sample_techproducts_configs
(Add core name in place of CORE_NAME in the command. Ex: core_test)
 -p 8983 is not necessery if we start the server in default port.
The output will be like this
Setup new core instance directory:
/apps/solr/solr-5.3.1/server/solr/core_test
Creating new core 'core_test' using command:
http://localhost:8983/solr/admin/cores?action=CREATE&name=core_test&instanceDir=core_test
{
  "responseHeader":{
    "status":0,
    "QTime":885},
  "core":"core_test"}
New core instance directory : solr-5.3.1/server/solr/CORE_NAME

# Modify schema.xml :
Directory of schema.xml is : /solr-5.3.1/server/solr/CORE_NAME/conf
Only file path indexing is needed, so unnecessary fileld could be avoided . Set these fileld tags as shown bellow :
 <field name="_version_" type="long" indexed="true" stored="true"/>
<field name="_root_" type="string" indexed="true" stored="false"/>
<field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" />
<field name="store" type="location" indexed="true" stored="true"/>
<field name="url" type="text_general" indexed="false" stored="false"/>
<field name="links" type="string" indexed="true" stored="true" multiValued="true"/>
 <field name="_src_" type="string" indexed="false" stored="true"/>
<field name="includes" type="text_general" indexed="true" stored="true" termVectors="true" termPositions="true" termOffsets="true" />
Set the value of indexed and stored as 'false' for rest of the field tags.

# Solr (.cal) file indexing :
Go to the Solr directory (Ex: /apps/solr/solr-5.3.1).
Now for indexing all the .cal/.txt files of a directory, execute the bellow command.
    nohup  java -Dauto -Dc=CORE_NAME -Dport=8983 -Drecursive -Dfiletypes=FILE_TYPE -jar example/exampledocs/post.jar DIRECTORY
Port is not necessary if default port is used.

# Solr commands :
To check if Solr is running or not, go to the following url : http://HOST:PORT/solr/
To start or stop Solr, go to Solr directory (Ex: /apps/solr/solr-5.3.1).
Redirect to bin : cd bin
Start solr : ./solr start
Stop solr : ./solr stop
Restart solr : ./solr restart
Delete core : ./solr delete -c $Core_Name [Ex: core_test]

