import org.cloudbus.cloudsim.*;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.*;
import java.util.*;

public class TwoUserSimulation {

    static Datacenter createDatacenter(String name) {

        List<Host> hostList = new ArrayList<>();

        List<Pe> peList = new ArrayList<>();
        peList.add(new Pe(0,new PeProvisionerSimple(1000)));

        Host host = new Host(
                0,
                new RamProvisionerSimple(2048),
                new BwProvisionerSimple(10000),
                1000000,
                peList,
                new VmSchedulerTimeShared(peList)
        );

        hostList.add(host);

        DatacenterCharacteristics characteristics =
                new DatacenterCharacteristics(
                        "x86","Linux","Xen",
                        hostList,
                        10.0,3.0,0.05,0.001,0.0
                );

        try {
            return new Datacenter(
                    name,
                    characteristics,
                    new VmAllocationPolicySimple(hostList),
                    new LinkedList<Storage>(),
                    0
            );
        }
        catch(Exception e){
            e.printStackTrace();
            return null;
        }
    }

    public static void main(String[] args) {

        try {

            CloudSim.init(2, Calendar.getInstance(), false);

            Datacenter dc1 = createDatacenter("Datacenter_1");
            Datacenter dc2 = createDatacenter("Datacenter_2");

            // USER 1
            DatacenterBroker broker1 = new DatacenterBroker("User1");
            int id1 = broker1.getId();

            Vm vm1 = new Vm(0,id1,500,1,512,1000,10000,"Xen",
                    new CloudletSchedulerTimeShared());

            broker1.submitVmList(Arrays.asList(vm1));

            UtilizationModel model = new UtilizationModelFull();

            Cloudlet c1 = new Cloudlet(0,20000,1,300,300,model,model,model);
            Cloudlet c2 = new Cloudlet(1,40000,1,300,300,model,model,model);

            c1.setUserId(id1);
            c2.setUserId(id1);

            broker1.submitCloudletList(Arrays.asList(c1,c2));


            // USER 2
            DatacenterBroker broker2 = new DatacenterBroker("User2");
            int id2 = broker2.getId();

            Vm vm2 = new Vm(1,id2,500,1,512,1000,10000,"Xen",
                    new CloudletSchedulerTimeShared());

            broker2.submitVmList(Arrays.asList(vm2));

            Cloudlet c3 = new Cloudlet(2,30000,1,300,300,model,model,model);
            Cloudlet c4 = new Cloudlet(3,60000,1,300,300,model,model,model);

            c3.setUserId(id2);
            c4.setUserId(id2);

            broker2.submitCloudletList(Arrays.asList(c3,c4));


            CloudSim.startSimulation();

            List<Cloudlet> r1 = broker1.getCloudletReceivedList();
            List<Cloudlet> r2 = broker2.getCloudletReceivedList();

            CloudSim.stopSimulation();


            System.out.println("\nUSER 1 RESULTS:");
            for(Cloudlet cl : r1)
                System.out.println("Cloudlet "+cl.getCloudletId()+" Time "+cl.getActualCPUTime());

            System.out.println("\nUSER 2 RESULTS:");
            for(Cloudlet cl : r2)
                System.out.println("Cloudlet "+cl.getCloudletId()+" Time "+cl.getActualCPUTime());

        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
}
