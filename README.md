parallel computing
2021/ICT/30

Date : 2026.03.06
==================================================
#include <mpi.h> 
#include <stdio.h> 
int main()
{
	int np;
	int pid;
	int sum = 0;
	MPI_Init(NULL, NULL);
	MPI_Comm_size(MPI_COMM_WORLD, &np);
	MPI_Comm_rank(MPI_COMM_WORLD, &pid);
	MPI_Status sta;

	if (pid == 0)
	{
		int num = 5;
		int recv_num3;
		MPI_Send(&num, 1, MPI_INT, pid + 1, 50, MPI_COMM_WORLD);
		printf("PID : %d, Sent value: %d\n", pid + 1, num);

		MPI_Recv(&recv_num3, 1, MPI_INT, np - 1, 70, MPI_COMM_WORLD, &sta);
		printf("PID: %d, received value : %d\n", pid - 1, recv_num3);


	}
	
	else if (pid != 0 && pid != np - 1) {
		int recv_num;
		//int sendnum;
		MPI_Recv(&recv_num, 1, MPI_INT, pid - 1, 50, MPI_COMM_WORLD, &sta);
		printf("PID: %d, received value: %d\n", pid - 1, recv_num);

		MPI_Send(&recv_num, 1, MPI_INT, pid + 1, 60, MPI_COMM_WORLD);
		printf("PID : %d, Sent value: %d\n", pid + 1, recv_num);
	}
	else {
		int recv_num2;
		//int sendnum2;
		MPI_Recv(&recv_num2, 1, MPI_INT, pid - 1, 60, MPI_COMM_WORLD, &sta);
		printf("PID: %d, received value: %d\n", pid - 1, recv_num2);

		MPI_Send(&recv_num2, 1, MPI_INT, 0, 70, MPI_COMM_WORLD);
		printf("PID : %d, Sent value: %d\n", pid == 0, recv_num2);
	}

	MPI_Finalize();
	return 0;
}



C:\Users\User\Desktop\2021ICT30\parallel com\06.03.2026\6ht_march\x64\Debug>mpiexec -n 3 6ht_march.exe
PID: 0, received value: 5
PID : 2, Sent value: 5
PID : 1, Sent value: 5
PID: -1, received value : 5
PID: 1, received value: 5
PID : 0, Sent value: 5
