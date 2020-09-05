machines = [
    {"name" => "master",    "ip" => "192.168.56.10", "cpus" => 2, "memory" => 2 * 1024, "playbook" => "master.yml"},
    {"name" => "node1", 	"ip" => "192.168.56.11", "cpus" => 2, "memory" => 2 * 1024, "playbook" => "node.yml"},
]

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.ssh.forward_agent = true
    config.ssh.insert_key = false

    # Build each vm
    machines.each do |item|
        config.vm.define "#{item['name']}" do |machine|
            machine.vm.network "private_network", ip: "#{item['ip']}"
            machine.vm.hostname = "#{item['name']}"

            # Set virtual machine name, cpu, memory and add hard drive (if applicable)
            machine.vm.provider :virtualbox do |vb|
                vb.name = "#{item['name']} - #{item['ip']}"
                vb.cpus = item['cpus']
                vb.memory = item['memory']
            end

            # Run ansible playbook
            machine.vm.provision "ansible_local" do |ansible|
                ansible.become = true
                ansible.playbook = "playbooks/#{item['playbook']}"
            end
        end
    end
end