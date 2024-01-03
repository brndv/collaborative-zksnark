# collaborative-zksnarks

This is a repo forked from [alex-ozdemir/collaborative-zksnark](https://github.com/alex-ozdemir/collaborative-zksnark) but a second demo computation(fibonacci) is added in the [collaborative-zksnark/mpc-snarks/src/proofs.rs](https://github.com/brndv/collaborative-zksnark/blob/main/mpc-snarks/src/proof.rs). And the script [collaborative-zksnark/blob/main/mpc-snarks/scripts/print.zsh](https://github.com/brndv/collaborative-zksnark/blob/main/mpc-snarks/scripts/print.zsh) is adapted to run the demo and print some details of the king node in the mpc-snarks process.

### Run The Demo

```bash
git clone https://github.com/brndv/collaborative-zksnark.git
cd collaborative-zksnark/mpc-snarks
cargo build --release --bin proof
./script/print.zsh fibonacci plonk spdz 10 3
```

about the ./script/print.zsh

```bash
computation=$1
proof=$2
infra=$3
size=$4
n_parties=$5

if [[ -z $BIN ]]
then
    BIN=./target/release/proof
fi
LABEL="timed section"

function usage {
  echo "Usage: $0 {squaring,fibonacci} {groth16,marlin,plonk} {hbc,spdz,gsz,local,ark-local} N_SQUARINGS N_PARTIES" >&2
  exit 1
}
```

$computation could be applied squaring which is the original demo used in  [alex-ozdemir/collaborative-zksnark](https://github.com/alex-ozdemir/collaborative-zksnark). $proof is the zk-snarks scheme applied.  $infra is the MPC scheme to be applied. $size is the computation size of corresponding computation length. $n_parties is to set the number of parties involved in the demo. 

$computation options : {squaring, fibonacci}

$proof options : {groth16, marlin, plonk}

<<<<<<< HEAD
$infra options:  {hbc, spdz, gsz, local, ark-local} (but ark-local not available when applying marlin in $proof)


=======
$infra options:  {hbc, spdz, gsz, local, ark-local} (but no ark-local when applying marlin in $proof)
>>>>>>> 6e34e0626d478333a34ba806edf1a7a283f873c5

### About the collaborative-snarks used in this demo

The collaborative-snarks used here involves:

1. Proving and Verifying parameters is set up of corresponding compuation circuit in the king node and publicized in the mpc net 
2. Circuit data (witness) are produced in the king node and the witness vectors are broken into a set of vectors (with the same number of elments as in the witness vectors) as in corresponding MPC scheme. Every node in the mpc net holds its unique(with high probability) share(vector) of the witness. Each of the nodes computes its commitments based on the publicized proving parameters with its share and broadcasts the result in the net.
3.  Every node in the MPC net can recover the final proof of the circuit data(witness) in step 2 produced in the king node according to the MPC scheme (SPDZ, GSZ, or HBC). And still the complete circuit data(witness) is only known by the king node after the MPC.

This approach can be well exploited to alleviate the computation burden in the king node(client) and still protect the privacy of the client data.
