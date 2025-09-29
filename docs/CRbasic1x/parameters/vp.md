# Vp

The Vp parameter can be used to either write the state of the V+ relay or read the state of the V+ relay (opened or closed). With the mask parameter set to Write, writing a 0 in the Vp parameter opens the V+ relay. Writing a 1 to the Vp parameter closes the V+ relay. The closed relay connects the V+ input to V+ output and is capable of passing up to 5A of current.With the mask set to Read, reading a 0 from the Vp parameter means the SIO2R is commanding the V+ relay to be open. Reading a 1 from the Vp parameter means the relay is being commanded to be closed. Note: This only reads the commanded state not the actual state. The relay contacts could permanently fail in closed or opened positions.

Type: Constant or Variable of Type Long
