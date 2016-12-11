# Security
> Coulouris 11, except 11.3.1-11.3.3 (inclusive).

The need for security mechanisms in distributed systems arises from the desire to share resources. Resources that are not shared can generally be protected by isolating them from external access.

A good starting point for identification of security requirements is:
- Processes encapsulate resources and allow clients to access them through interfaces.
	-	Users or other processes are authorized to operate on resources.
	- Resources must be protected against unauthorized access.
- Processes interact through a network that is shared by many users.
	-	Enemies can access the network. They can copy or attempt to read any message transmitted through the network and they can inject arbitrary messages, addressed to any destination and purporting to come from any source, into the network.

### The role of cryptography
Digital cryptography provides the basis for most computer security mechanisms.

Cryptography is the art of encoding information in a format that only the intended recipients can decode.

Cryptography can also be employed to provide proof of the authenticity of information, analogous to the use of signatures in conventional transactions.

### Familiar names for *participants* in security protocols
<table>
	<tr>
		<td>Name</td>
		<td>Role</t>
	</tr>
	<tr>
		<td>Alice</td>
		<td>First participant</t>
	</tr>
	<tr>
		<td>Bob</td>
		<td>Second participant</t>
	</tr>
	<tr>
		<td>Carol</td>
		<td>Participant in three- and four-party protocols.</t>
	</tr>
	<tr>
		<td>Dave</td>
		<td>Participant in four-party protocols</t>
	</tr>
	<tr>
		<td>Eve</td>
		<td>Eavesdropper</t>
	</tr>
	<tr>
		<td>Mallory</td>
		<td>Malicious attacker</t>
	</tr>
	<tr>
		<td>Sara</td>
		<td>A server</t>
	</tr>
</table>

### Threats and attacks
Security threats fall into 3 broad classes:
- *Leakage*: Refers to the acquisition of information by unauthorized recipients.
- *Tampering*: Refers to the unauthorized alteration of information.
- *Vandalism*: Refers to interference with the proper operation of a system without gain to the perpetrator.

Attacks on distributed systems depend upon obtaining access to existing communication channels or establishing new channels that masquerade as authorized connections.
- *Eavesdropping*: Obtaining copies of messages without authority.
- *Masquerading*: Sending or receiving messages using the identity of another principal without their authority.
- *Message tampering*: Intercepting messages and altering their contents before passing them on to the intended recipient.
	- One example of this is *man-in-the-middle* attacks where the very first message in an exchange of encryption keys to establish a secure connection is intercepted. The attacker substitutes compromised keys that enable them to decrypt subsequent messages before re-encrypting them in the correct keys and passing them on.
- *Replaying*: Storing intercepted messages and sending them at a later date. This attack may be effective even with authenticated and encrypted messages.
- *Denial of service*: Flooding a channel or other resource with messages in order to deny access for others.

### Information Leakage
If the transmission of a message between two processes can be observed, some information can be gleaned from its mere existence.

### Overview of security techniques
When designing for security, it is necessary to assume the worst.

- **Worst-case assumptions and design guidelines**:
	-	**Interfaces are exposed**: Distributed systems are composed of processes that offer services or share information. Their communication interfaces are necessarily open to allow new clients to access them. An attacker can send a message to any interface.
	- **Networks are insecure**: For example, message sources can be falsified. Messages can be made to look as though they came from Alice when they were actually sent by Mallory. Host addresses can be 'spoofed' - Mallory can connect to the network with the same address as Alice and receive copies of messages intended for her.
	- **Limit the lifetime and scope of each secret**: When a secret key is first generated, we can be confident that it has not been compromised. **The longer we use it, and the more widely it is known, the greater the risk. The use of secrets such as passwords and shared secret keys should be time-limited and sharing should be restricted.**
	- **Algorithms and program code are available to attackers**: The bigger and the more widely distributed a secret is, the greater the risk of its disclosure. Secret encryption algorithms are totally inadequate for today's large-scale network environments.

	// TODO: Read the rest of the chapter & Write notes!
