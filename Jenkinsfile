pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    parameters {
        choice(choices: ['MySql','MSSQL', 'Postgres', 'Oracle','spark-dev', 'Commvault', 'cyberark3', 'Oracle-demo','k8s', 'Oracle-rac'], description: 'Select the Solution to build', name: 'solution')
        //choice(choices: ['cowriter','MySql','MSSQL', 'MSSQLDC', 'Postgres', 'Oracle','winjump','logrhythm','syslog','qradar','superna','superna-ubuntu','util','k8s', 'Oracle-rac', 'splunk', 'superna-windows','superna-windows2','superna-windows3','superna-windows-19','akriti-ubuntu', 'linux-ubuntu', 'spark', 'cyberark', 'cyberark1', 'cyberark2', 'cyberark3', 'spark-dev', 'veeam-backup-and-replication','cyberark-pvwa', 'veeam'], description: 'Select the Solution to build', name: 'solution')
        string(name: 'count', defaultValue: "0", description: 'Number of VMs')
        choice(choices: ['fsvc', 'shared-vc'], description: 'Select the VC to use', name: 'vcenter')
        booleanParam(name: 'Build', defaultValue: false, description: 'Build Intrastructure')
        booleanParam(name: 'Install', defaultValue: false, description: 'Install and configure solution')
	    booleanParam(name: 'Test', defaultValue: false, description: 'Run the performance test')
	    booleanParam(name: 'Destroy', defaultValue: false, description: 'Destroy Intrastructure')
		
    }

    stages {
        stage('Build solution') {
            environment {
                SSH_KEY = credentials('ansible')
		WINDOWS_ADMIN_PASS = credentials('windows_admin_password')
                VC_PASS = credentials("${params.vcenter}")
                INFOBLOX_PASS = credentials('infoblox')
                AWS_ACCESS_KEY_ID = 'PSFBSAZRAECJNHNFJEKCPOHOOPMGMKMJLIJLKBCMLB'
                AWS_SECRET_ACCESS_KEY = credentials('s3token')
                ANSIBLE_HOST_KEY_CHECKING = "False"
                ANSIBLE_ROLES_PATH = "../../ansible/roles"
                vm_count = "${params.count}".toInteger()
            }
            steps {
                
                script {
    		        sh "echo Hello from Build stage"
                    //sh 'echo ssh key from script section - ${SSH_KEY}'
    		        sol_name = params.solution
    		        build_solution(sol_name)
    	         }
                  }
        }
    }
}


  def build_solution(sol) {
    def tf_cmd = "/usr/bin/terraform"
	def workspace = pwd()
	println "workspace ------${workspace}-----"
	
	def solname = sol.trim()
	def path = workspace + "/" + "modules" + "/" + solname
	println "path ------${path}-----"
	dir(path) {

	    if (params.Build) {
              if (solname == 'veeam') {
/*
		dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-setup") {
                  println  "Setting Veeam Setup VM"
                  def vpath = workspace + "/" + "modules" + "/" + "veeam-setup".trim()
		  echo "current working directory: ${pwd()}"
		  println "vpath ------${vpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veem-setup"+"/g' backend.tf"
		  println "Executing Infrstructure build step" 
            	  sh script: "/bin/rm -rf .terraform"
	          print  "sh script: ${tf_cmd} init -upgrade"
	          sh script: "${tf_cmd} init -upgrade"
            	  count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	  //count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	  println count
            	  println vm_count
            	  total_count = vm_count.toInteger() + count.toInteger()
            	  println total_count
		  sh script: "$tf_cmd apply -auto-approve -var-file=$vpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	  sh script: "python3.9 ../../build-inventory.py " + "veeam-setup"
            	  sh script: "cat hosts.ini"
               }

	       dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-windows-backupproxy-server") {
            	  println  "Creating: Veeam - Windows BackUp Proxy Server"
                  def vwpath = workspace + "/" + "modules" + "/" + "veeam-windows-backupproxy-server".trim()
		  echo "current working directory: ${pwd()}"
		  println "vwpath ------${vwpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veeam-windows-backupproxy-server"+"/g' backend.tf"
		  println "Executing Infrstructure build step" 
            	  sh script: "/bin/rm -rf .terraform"
	          print  "sh script: ${tf_cmd} init -upgrade"
	          sh script: "${tf_cmd} init -upgrade"
            	  count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	  //count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	  println count
            	  println vm_count
            	  total_count = vm_count.toInteger() + count.toInteger()
            	  println total_count
		  sh script: "$tf_cmd apply -auto-approve -var-file=$vwpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	  sh script: "python3 ../../build-inventory.py " + "veeam-windows-backupproxy-server"
            	  sh script: "cat hosts.ini"
	       }
*/
      	       dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-linux-backupproxy-server") {
            	  println  "Creating: Veeam - Linux BackUp Proxy Server"
                  def vlpath = workspace + "/" + "modules" + "/" + "veeam-linux-backupproxy-server".trim()
        	  echo "Inside Dir: ${pwd()}"
		  println "vlpath ------${vlpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veeam-linux-backupproxy-server"+"/g' backend.tf"
		  println "Executing Infrstructure build step" 
            	  sh script: "/bin/rm -rf .terraform"
	          print  "sh script: ${tf_cmd} init -upgrade"
	          sh script: "${tf_cmd} init -upgrade"
            	  count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	  //count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	  println count
            	  println vm_count
            	  total_count = vm_count.toInteger() + count.toInteger()
            	  println total_count
		  sh script: "$tf_cmd apply -auto-approve -var-file=$vlpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	  sh script: "python3.6 ../../build-inventory.py " + "veeam-linux-backupproxy-server"
            	  sh script: "cat hosts.ini"
	      }

              dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-windows-repo-server") {
            	  println  "Creating: Veeam - Windows Repo Server"
                  def vwpath = workspace + "/" + "modules" + "/" + "veeam-windows-repo-server".trim()
		  echo "current working directory: ${pwd()}"
		  println "vwpath ------${vwpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veeam-windows-repo-server"+"/g' backend.tf"
		  println "Executing Infrstructure build step" 
            	  sh script: "/bin/rm -rf .terraform"
	          print  "sh script: ${tf_cmd} init -upgrade"
	          sh script: "${tf_cmd} init -upgrade"
            	  count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	  //count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	  println count
            	  println vm_count
            	  total_count = vm_count.toInteger() + count.toInteger()
            	  println total_count
		  sh script: "$tf_cmd apply -auto-approve -var-file=$vwpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	  sh script: "python3 ../../build-inventory.py " + "veeam-windows-repo-server"
            	  sh script: "cat hosts.ini"
	       }

      	       dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-linux-repo-server") {
            	  println  "Creating: Veeam - Linux Linux Repo Server"
                  def vlpath = workspace + "/" + "modules" + "/" + "veeam-linux-repo-server".trim()
        	  echo "Inside Dir: ${pwd()}"
		  println "vlpath ------${vlpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veeam-linux-repo-server"+"/g' backend.tf"
		  println "Executing Infrstructure build step" 
            	  sh script: "/bin/rm -rf .terraform"
	          print  "sh script: ${tf_cmd} init -upgrade"
	          sh script: "${tf_cmd} init -upgrade"
            	  count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	  //count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	  println count
            	  println vm_count
            	  total_count = vm_count.toInteger() + count.toInteger()
            	  println total_count
		  sh script: "$tf_cmd apply -auto-approve -var-file=$vlpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	  sh script: "python3.6 ../../build-inventory.py " + "veeam-linux-repo-server"
            	  sh script: "cat hosts.ini"
	      }

	    } else {
            	println "Updating backend file"
            	sh script: "sed -i -e 's/sol_name/"+solname+"/g' backend.tf"
			println "Executing Infrstructure build step" 
            	sh script: "/bin/rm -rf .terraform"
	        print  "sh script: ${tf_cmd} init -upgrade"
	        sh script: "${tf_cmd} init -upgrade"
            	count = sh(script: "grep vm_count main.tfvars | awk  '{print \$3}' |xargs", returnStdout: true)
            	//count = sh(script: "cat hosts.ini|wc -l", returnStdout: true)
           	 println count
            	println vm_count
            	def total_count = vm_count.toInteger() + count.toInteger()
            	println total_count
		sh script: "$tf_cmd apply -auto-approve -var-file=$path"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	 + " -var ansible_key=" + '${SSH_KEY}'	+	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'  +	" -var vm_count=" + total_count
            	sh script: "python3 ../../build-inventory.py " + solname
            	sh script: "cat hosts.ini"
	   }
        }
        if (params.Install) {
	    println "Installing and conifguring the solution"
            println solname
            println "------------------"
            if (solname == 'MSSQLDC') {
                sh script: "ansible-playbook -i hosts.ini ../../ansible/playbooks/" +  "common-win.yml"
                sh script: "ansible-playbook -i hosts.ini ../../ansible/playbooks/" + solname.toLowerCase() + "-install.yml"
            } 
            if  (solname == 'Oracle') {
                sh script: "cd /root/COPY_OF_ORACLE_BUILD/ansible; export ANSIBLE_COLLECTIONS_PATHS=/root/.ansible/collections; export ANSIBLE_ROLES_PATH=/root/.ansible/collections/ansible_collections/opitzconsulting/ansible_oracle/roles; export ANSIBLE_PYTHON_INTERPRETER=/usr/bin/python3.6; ansible-playbook -i inventory-asm-demo -e hostgroup=dbfs playbooks/single-instance-asm.yml --private-key "  + '${SSH_KEY}' + " --user ansible  -v"
            }
            if  (solname == 'veeam') {
		dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-setup") {
                  def vpath = workspace + "/" + "modules" + "/" + "veeam-setup".trim()
		  println "vpath ------${vpath}-----"
		  println "Windows_Admin_Pass ------${WINDOWS_ADMIN_PASS}-----"
                  sh script: "ansible-galaxy collection install veeamhub.veeam"
                  sh script: "cat hosts.ini"
               	  sh script: "echo [veeam-server] > inventory.ini"
                  sh script: "cat hosts.ini >> inventory.ini"
               	  sh script: "echo [veeam-win-proxy-server] >> inventory.ini"
                  sh script: "cat ../veeam-windows-repo-server/hosts.ini >> inventory.ini"
               	  sh script: "echo [veeam-linux-proxy-server] >> inventory.ini"
                  sh script: "cat ../veeam-linux-repo-server/hosts.ini >> inventory.ini"
                  sh script: "cat inventory.ini"
               	  sh script: "ansible-playbook -i inventory.ini ../../ansible/playbooks/" +  "veeam-install.yml" + " -e 'ansible_user=Administrator ansible_password=${WINDOWS_ADMIN_PASS} ansible_connection=winrm ansible_shell_type=cmd ansible_port=5985 ansible_winrm_transport=ntlm ansible_winrm_server_cert_validation=ignore ansible_winrm_scheme=http ansible_winrm_kerberos_delegation=true'" 
                  veeam_windows_proxy_server = sh(script: 'head -n 1 /var/lib/jenkins/workspace/Solution-automation/modules/veeam-windows-backupproxy-server/hosts.ini')
           	  println veeam_windows_proxy_server
               	  sh script: "ansible-playbook -i inventory.ini ../../ansible/playbooks/" +  "veeam-windows-proxy-server.yml" + " -e 'ansible_user=Administrator ansible_password=${WINDOWS_ADMIN_PASS} ansible_connection=winrm ansible_shell_type=cmd ansible_port=5985 ansible_winrm_transport=ntlm ansible_winrm_server_cert_validation=ignore ansible_winrm_scheme=http ansible_winrm_kerberos_delegation=true veeam_windows_proxy_server=10.21.210.142'" 

                  // veeam_windows_repo_server = sh(script: 'head -n 1 /var/lib/jenkins/workspace/Solution-automation/modules/veeam-windows-repo-server/hosts.ini')

           	  // println veeam_windows_repo_server
              	  // sh script: "ansible-playbook -i inventory.ini ../../ansible/playbooks/" +  "veeam-windows-repo-server.yml" + " -e 'ansible_user=Administrator ansible_password=${WINDOWS_ADMIN_PASS} ansible_connection=winrm ansible_shell_type=cmd ansible_port=5985 ansible_winrm_transport=ntlm ansible_winrm_server_cert_validation=ignore ansible_winrm_scheme=http ansible_winrm_kerberos_delegation=true veeam_windows_repo_server=10.21.210.73'" 
              }
            }
            else {
            //    sh script: "ansible-playbook -i hosts.ini ../../ansible/playbooks/" +  "common.yml --private-key "  + '${SSH_KEY}' + " --user ansible"
            //    sh script: "ansible-playbook -i hosts.ini ../../ansible/playbooks/" + solname.toLowerCase() + "-install.yml --private-key "  + '${SSH_KEY}' + " --user ansible"
            }
                
           
			
        }
        if (params.Test) {
			println "Executing Performance step"
            sh script: "ansible-playbook -i hosts.ini ../../ansible/playbooks/" + solname.toLowerCase() + "-test.yml --private-key "  + '${SSH_KEY}' + " --user ansible"
        }

        if (params.Destroy) {
	      if (solname == 'veeam') {
		dir("/var/lib/jenkins/workspace/Solution-automation/modules/veeam-setup") {
                  println  "Destroying Veeam Setup"
                  def vpath = workspace + "/" + "modules" + "/" + "veeam-setup".trim()
		  echo "current working directory: ${pwd()}"
		  println "vpath ------${vpath}-----"
	 	  println "Updating backend file"
            	  sh script: "sed -i -e 's/sol_name/"+"veem-setup"+"/g' backend.tf"
                  sh script: "${tf_cmd} init -reconfigure"
	          sh script: "${tf_cmd} destroy -auto-approve -var-file=$vpath"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	+ " -var ansible_key=" + '${SSH_KEY}'	 +	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'	 +	" -var vm_count=" + '${vm_count}'	
               }

            } else {
                println "Executing Infrstructure destroy step" 
                sh script: "sed -i -e 's/sol_name/"+solname+"/g' backend.tf"
                sh script: "${tf_cmd} init -reconfigure"
			    sh script: "${tf_cmd} destroy -auto-approve -var-file=$path"  + "/main.tfvars" + " -var vsphere_password=" + '${VC_PASS}'	+ " -var ansible_key=" + '${SSH_KEY}'	 +	 " -var infoblox_pass=" + '${INFOBLOX_PASS}'	 +	" -var vm_count=" + '${vm_count}'	
            }
			
        }

	}
	
	
	
	
  }
      
