# `libp2p`


### pseudo-code for test shared by @mxinden

> > hi @mxinden, could you give me some pointers on how to come up with a test for this scenario?
> 
> Instead of extending an existing test, I would suggest creating a new one, only including a very minimal setup. Is the pseudo code below of some help @whereistejas?
> 
> diff --git a/protocols/request-response/tests/ping.rs b/protocols/request-response/tests/ping.rs
> index b03931cd..5622fb06 100644
> --- a/protocols/request-response/tests/ping.rs
> +++ b/protocols/request-response/tests/ping.rs
> @@ -274,6 +274,28 @@ fn ping_protocol_throttled() {
>      pool.run_until(peer2);
>  }
>
```rust
#[test]
fn is_pending_outbound() {
    let ping = Ping("ping".to_string().into_bytes());
    let offline_peer = PeerId::random();

    let protocols = iter::once((PingProtocol(), ProtocolSupport::Full));
    let cfg = RequestResponseConfig::default();

    let (peer1_id, trans) = mk_transport();
    let ping_proto1 = RequestResponse::throttled(PingCodec(), protocols.clone(), cfg.clone());
    let mut swarm1 = Swarm::new(trans, ping_proto1, peer1_id.clone());

    let request_id_1 = swarm1.send_request(offline_peer, ping.clone());

    // Poll swarm until request 1 is reported as failed.

    let request_id_2 = swarm1.send_request(offline_peer, ping.clone());

    assert!(!swarm1.is_pending_outbound(offline_peers, request_id_1));
    assert!(swarm1.is_pending_outbound(offline_peers, request_id_2));
}
```
