# Building Battle-Tested Network Transport

<p><em>Authored by Rajiv Puvvada, Senior Software Development Engineer, Amazon Lumberyard.</em></p> 
       <p><img class="aligncenter size-full wp-image-3386" src="/blogs/building-battle-tested-network-transport/images/Networking-blog-LY-1.png" alt="" width="792" height="439"></p> 
       <h2>Introduction</h2> 
       <p>Online multiplayer isn’t just a feature for many games; it’s core component of gameplay. It has to be performant; it has to be reliable; and it has to be stable. And, perhaps to no-one’s surprise, developing online multiplayer is hard — especially when poor design or implementation can, well, ruin that gameplay. Multiplayer games have been growing and evolving in recent years and it’s not unreasonable to think they’ll continue to — people like to play together, and they like new experiences. As a result, players’ and customers’ expectations for networking reliability and performance have grown as well.</p> 
       <p>It’s why we wanted to take a step back re-evaluate our Networking feature in <a href="https://aws.amazon.com/lumberyard/" target="_blank" rel="noopener noreferrer">Amazon Lumberyard</a> and its ability to meet the increasing demands of online gaming. We decided to start with the backbone of online networking, the transport layer, and how we implemented it previously in our GridMate networking component.</p> 
       <h3>Looking back at GridMate</h3> 
       <p>GridMate has a fairly robust transport layer. It implements features such as:</p> 
       <ul> 
        <li>Ordered/unordered messaging</li> 
        <li>Reliable/unreliable messaging</li> 
        <li>Fragmenting messages based on a prescribed MTU (Maximum Transmission Unit)</li> 
        <li>DTLS/TLS encryption</li> 
        <li>Opaque compression using a compression library like zlib</li> 
        <li>Prescriptive bitpacking using supplied Marshallers</li> 
        <li>Channels on which to divide different types of traffic</li> 
        <li>Integration with <a href="https://aws.amazon.com/gamelift/" target="_blank" rel="noopener noreferrer">Amazon GameLift</a></li> 
       </ul> 
       <p>Overall, it was pretty worthy, even now. But that’s not to say we didn’t identify potential areas of improvement as well! For example, we found the following limitations in GridMate:</p> 
       <ul> 
        <li>Hard-baked limitations on the amount of data and number of components that can be synchronized</li> 
        <li>Hard-baked limitation on the number of players that can connect to a host</li> 
        <li>Head of line blocking caused by ordered packet delivery</li> 
        <li>Lengthy implementations that we felt could be streamlined and condensed</li> 
        <li>API rigidity making it difficult for a developer to do things like easily add custom handshake logic</li> 
       </ul> 
       <p>GridMate is definitely a solid networking library. That being said, it also has room for improvement and is built primarily for single-server multiplayer games. With that in mind, we decided to start fresh and build a new streamlined transport layer with the flexibility and performance to handle a wider range of networking needs.</p> 
       <h2>Looking forward to AzNetworking</h2> 
       <p><code>AzNetworking</code> is a new core Lumberyard API for our new transport layer. As our guiding principles in its development, we determined a new networking library must be:</p> 
       <ul> 
        <li>Streamlined – The API should be readable, well-documented, and concise.</li> 
        <li>Performant – Transport is all about sending and receiving the most amount of meaningful data as quickly as possible and in as little space as possible. All of this should be quantifiable and measurable.</li> 
        <li>Battle-tested – Transport should be able to maintain exceptionally long uptimes with no service failures or degradation.</li> 
       </ul> 
       <p>So we built it. Amongst other things, AzNetworking provides the means to:</p> 
       <ul> 
        <li>Create a listen host capable of accepting remote connections, either using TCP or UDP.</li> 
        <li>Connect a client process to a listen host, either using TCP or UDP.</li> 
        <li>Send unreliable and unordered packets, on TCP this falls back to a reliable ordered packet send.</li> 
        <li>Send reliable packets. With UDP these may be received out of order, but TCP will guarantee ordering.</li> 
        <li>Fragment packets over MTU.</li> 
        <li>DTLS encryption for UDP connections and TLS for TCP via OpenSSL.</li> 
        <li>Query if a given unreliable packet was received by the remote endpoint.</li> 
        <li>Provide and use multiple traffic compressors as plug-ins using a very similar interface to GridMate.</li> 
        <li>Measure network diagnostics via built in metrics.</li> 
        <li>Simulate network conditions including artificial latency, packet loss rate, and latency variance via configuration.</li> 
        <li>Specify the structure of packets via XML and generate corresponding code using jinja templating</li> 
        <li>Prescriptively serialize and bitpack by specifying custom serializers.</li> 
        <li>Create multiple <code>NetworkInterfaces</code> to serve specific types of traffic including loopback traffic.</li> 
       </ul> 
       <p>How did we do? Let’s gauge with some easy to understand, measurable and battle-tested data.</p> 
       <h4>Code streamlining</h4> 
       <p>First, we wanted to reduce the overall codebase. We measured code size by counting lines in .cpp, .h, .inl and .jinja files in both libraries, excluding test collateral but including code generated from jinja templates.</p> 
       <ul> 
        <li>GridMate: 37419 lines of code</li> 
        <li>AzNetworking: 13405 lines of code</li> 
       </ul> 
       <p>That’s nearly a 3x reduction in code, which means it will be easier to maintain overall.</p> 
       <p>Now, let’s take a look at <code>InitiateConnectionPacket</code>, an API for one of our core packet types, to see what the generated code from a packet’s XML definition looks like.&nbsp;Here’s <code>InitiateConnectionPacket</code>’s XML specification. It’s a straightforward packet containing a buffer for any initial handshake data:</p> 
       <pre><code class="lang-xml">&lt;Packet Name="InitiateConnectionPacket" Desc="This packet is used to initiate a new connection"&gt;
    &lt;Member Type="AzNetworking::UdpPacketEncodingBuffer" Name="handshakeBuffer" /&gt;
&lt;/Packet&gt;</code></pre> 
       <p>The generated header for <code>InitiateConnectionPacket</code>&nbsp;looks like this:</p> 
       <pre><code class="lang-xml">//! @class InitiateConnectionPacket
//! @brief This packet is used to initiate a new connection.
class InitiateConnectionPacket final
    : public AzNetworking::IPacket
{
public:
    static constexpr AzNetworking::PacketType Type = aznumeric_cast&lt;AzNetworking::PacketType&gt;(PacketType::InitiateConnectionPacket);

    InitiateConnectionPacket() = default;
    explicit InitiateConnectionPacket
    (
        AzNetworking::UdpPacketEncodingBuffer handshakeBuffer
    );
    ~InitiateConnectionPacket() override = default;

    //! Equality operator, returns true if the current instance is equal to rhs.
    //! @param rhs the InitiateConnectionPacket instance to test for equality against
    //! @return boolean true if equal, false if not
    bool operator ==(const InitiateConnectionPacket&amp; rhs) const;

    //! Inequality operator, returns true if the current instance is not equal to rhs.
    //! @param rhs the InitiateConnectionPacket instance to test for inequality against
    //! @return boolean false if equal, true if not equal
    bool operator !=(const InitiateConnectionPacket&amp; rhs) const;

    //! Sets the value of handshakeBuffer.
    //! @param value the value to set handshakeBuffer to
    void SetHandshakeBuffer(const AzNetworking::UdpPacketEncodingBuffer&amp; value);

    //! Gets the value of handshakeBuffer.
    //! @return the value of handshakeBuffer
    const AzNetworking::UdpPacketEncodingBuffer&amp; GetHandshakeBuffer() const;

    //! Retrieves a non-const reference to the value of handshakeBuffer
    //! @return a non-const reference to the value of handshakeBuffer
    AzNetworking::UdpPacketEncodingBuffer&amp; ModifyHandshakeBuffer();

    //! IPacket interface
    //! @{
    AzNetworking::PacketType GetPacketType() const override;
    AZStd::unique_ptr&lt;AzNetworking::IPacket&gt; Clone() const override;
    bool Serialize(AzNetworking::ISerializer&amp; serializer) override;
    //! @}

private:

    AzNetworking::UdpPacketEncodingBuffer m_handshakeBuffer;
};</code></pre> 
       <p>In the corresponding <code>.cpp</code>&nbsp;file you’ll find most of the meat including serialization:</p> 
       <pre><code class="lang-xml">InitiateConnectionPacket::InitiateConnectionPacket
(
    AzNetworking::UdpPacketEncodingBuffer handshakeBuffer
)
    : m_handshakeBuffer(handshakeBuffer)
{
    ;
}

AzNetworking::PacketType InitiateConnectionPacket::GetPacketType() const
{
    return Type;
}

bool InitiateConnectionPacket::operator ==([[maybe_unused]] const InitiateConnectionPacket&amp; rhs) const
{
    if (m_handshakeBuffer != rhs.m_handshakeBuffer)
    {
        return false;
    }
    return true;
}

AZStd::unique_ptr&lt;AzNetworking::IPacket&gt; InitiateConnectionPacket::Clone() const
{
    AZStd::unique_ptr&lt;InitiateConnectionPacket&gt; result = AZStd::make_unique&lt;InitiateConnectionPacket&gt;();
    result-&gt;m_handshakeBuffer = m_handshakeBuffer;
    return result;
}

bool InitiateConnectionPacket::Serialize(AzNetworking::ISerializer&amp; serializer)
{
    serializer.Serialize(m_handshakeBuffer, "handshakeBuffer");
    return serializer.IsValid();
}</code></pre> 
       <p>Last but not least we have the inline methods:</p> 
       <pre><code class="lang-xml">inline bool InitiateConnectionPacket::operator !=(const InitiateConnectionPacket &amp;rhs) const
{
&nbsp;&nbsp; &nbsp;return !(*this == rhs);
}

inline void InitiateConnectionPacket::SetHandshakeBuffer(const AzNetworking::UdpPacketEncodingBuffer&amp; value)
{
&nbsp;&nbsp; &nbsp;m_handshakeBuffer = value;
}

inline const AzNetworking::UdpPacketEncodingBuffer&amp; InitiateConnectionPacket::GetHandshakeBuffer() const
{
&nbsp;&nbsp; &nbsp;return m_handshakeBuffer;
}

inline AzNetworking::UdpPacketEncodingBuffer&amp; InitiateConnectionPacket::ModifyHandshakeBuffer()
{
&nbsp;&nbsp; &nbsp;return m_handshakeBuffer;
}</code></pre> 
       <p>As you can see a little bit of XML handles a fair bit of boilerplate.</p> 
       <h4>Battle-testing</h4> 
       <p>GridMate was largely tested through the use of MultiplayerSample, a 2D shoot-em-up (colloquially known as a “shmup”) game. With <code>AzNetworking</code> we opted to make the first major entry into our test suite a soak test. A “soak test” is a transport-oriented test designed to saturate a connection with traffic to observe the performance of the transport layer. The idea is to saturate a connection for hours on end to see what breaks or if degradation occurs.</p> 
       <p>Combined with the configurability of <code>AzNetworking</code>, we can selectively soak individual features and combinations of features. Our most robust configuration so far features:</p> 
       <ul> 
        <li>DTLS/TLS (depending on the protocol specified for the test)</li> 
        <li>Compression via zlib</li> 
        <li>Reliable packets sent every tick</li> 
        <li>10% chance of sending a 6KB packet every tick (well over our MTU to test fragmentation)</li> 
        <li>10% chance of sending an unreliable packet every tick</li> 
        <li>Artificial latency with variance and packet loss</li> 
       </ul> 
       <p>We run the soak test almost habitually at this point. Added a new feature? Fixed a transport bug? Just have some down time? The soak test is probably running. Thanks to its usage we’ve successfully hardened the API against a variety of issues and can now run it fully featured overnight.</p> 
       <h4>Performance</h4> 
       <p>We measured performance by having GridMate and <code>AzNetworking</code> each send a reliable one byte packet and profiling the send and receive portions of each’s transport. The scenario uses one client and one server with no compression or encryption.</p> 
       <ul> 
        <li>GridMate: ~50 microseconds</li> 
        <li><code>AzNetworking</code>: ~35 microseconds</li> 
       </ul> 
       <p>This is roughly a 30% improvement over the performance in GridMate!</p> 
       <p>(As a side note, with all features on profiling shows ~55 microseconds on average spent in <code>AzNetworking</code>’s Send/Receive logic.)</p> 
       <p>To this day, we’ve seen GridMate used to power a lot of impressive simulations and we wouldn’t be here without the lessons learned from it. We’re hoping the improvements demonstrated so far in <code>AzNetworking</code> can not only take the types of network simulations we’ve seen so far a step further but also power what comes next.</p> 
       
