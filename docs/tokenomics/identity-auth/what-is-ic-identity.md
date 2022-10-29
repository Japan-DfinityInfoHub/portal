# Internet Identity とは？

*Internet Identity* は、Internet Computer がサポートする匿名のブロックチェーン認証フレームワークです。 認証するためには、*Identity Anchor* を指定する必要があります。 ユーザーは ID の "アンカー" を作成し、ノート PC の指紋センサー、携帯電話の フェイス ID システム、YubiKey や Ledger ウォレットなどのポータブル HSM など、互換性のある暗号化デバイスを割り当てることができます。 その後、Internet Computer 上で動作する任意の Dapp に登録し、アンカーに割り当てられた任意のデバイスを使って認証することができます。これにより、ユーザーは最高レベルの暗号セキュリティの恩恵を受けながらも、自分自身で暗号鍵を直接管理する必要がなく、ミスや秘密鍵の盗難を防ぐことができ、非常に少ない労力で Dapps を認証することができるという高い利便性を実現しています。 このシステムは Dapp に対して匿名化されており、アンカーが Dapp とのやり取りに使われるたびに、Dapp は特別に生成された *偽名* を見ることになり、ユーザーが使用する様々な Dapp 間で追跡されることを防ぎます。また、Identity Anchor は好きなだけいくつでも作成することができます。

一般的な認証方法とは異なり、Internet Identity では、自分でパスワードを設定・管理したり、個人識別情報を Dapps や Internet Identity に提供する必要はありません。

## Internet Identity の仕組み

Internet Identity は、最新の Web ブラウザや OS でサポートされている Web 認証 (WebAuthn) API と、Internet Computer を支えるチェーンキー暗号のフレームワークをベースに構築されています。基本的に、Internet Computer はマスターチェーンキーを使用して、各アンカーに割り当てられたデバイス内の公開キーのリストに署名します。このマスターチェーンキーは、ウェブブラウザで実行されるクライアントサイドのコードなどで認識されます。

Internet Identity を組み込んだ Dapps では、Identity Anchor を使用して認証を行うよう促されます。 まだ Identity Anchor を持っていない場合は、簡単に作成して認証方法を追加することができます。 詳細については、[Internet Identity の使い方](auth-how-to)を参照してください。 追加したデバイスごとに、暗号鍵のペア(秘密鍵と公開鍵)が生成されます。 公開鍵は Internet Computer のブロックチェーンに保存され、秘密鍵は認証デバイスの中にアクセスを管理する生体認証データとともに保管されます。 Identity Anchorに複数の認証方法を追加すると、すべてのデバイスで Dapps にアクセスできるようになります。

認証に Internet Identity を使用する Dapp にアクセスする際には、まず使用する Identity Anchor を指定します。 設定されたデバイスを使って Identity Anchor を使用して認証すると、ブラウザは Internet Identity に接続し、その Dapp で使用するキーを生成します。 最後に、その Dapp へのアクセスを承認するように画面上で求められます。

ブラウザは認証をダウンロードしてから、Dapp にリダイレクトします。 Dapp は Internet Identity からの認証を確認し、アプリケーション固有の匿名 ID としてアクセスを許可します。これを偽名と呼びます。 内部的には、あなたは各 Dapp に対して異なる偽名を持っていますが、単一の Dapp に対する偽名はすべてのデバイスで同じです。 すべてのデバイスは、Identity Anchor の認証に使用できるさまざまな方法だけをあなたに示します。

Identity Anchor は、冗長性などのために必要な数だけ登録することができます。例えば、あるユーザーはSocialFi や GameFiで使う Anchor と、DeFi のみで使う Anchor を作成することができます。例えば、SocialFi や GameFi の Anchor には顔認識機能のみを追加し、DeFi 用の Anchor には YubiKeys や Ledger ウォレットなど、より安全なポータブル HSM デバイスのみを使用することを好むかもしれません。

## Internet Identity の使い方

Identity Anchor の作成と使用方法を順を追って学ぶためには、[Internet Identity の使い方](./auth-how-to.md)をご覧ください。 また、Identity Anchors のリカバリーに関する設定方法についても説明しています。

<!--
# What is Internet Identity?

*Internet Identity* is an anonymous blockchain authentication framework supported by the Internet Computer. Users can create identity "anchors" to which they assign compatible cryptographically enabled devices, such as the fingerprint sensor on a laptop, the face ID system on a phone, or a portable HSM, such as a YubiKey or Ledger wallet. Thereafter, they can signup and authenticate to any dapp running on the Internet Computer using any of the devices they have assigned to their anchor. This provides a high level of convenience, allowing users to authenticate to dapps they are interested in with a very low level of friction, while benefiting from the highest level of cryptographic security, but without the need to directly manage or handle cryptographic key material themselves, which prevents mistakes and the theft of their key material. The system is anonymizing towards dapps, and whenever an anchor is used to interact with a dapp, the dapp sees a specially generated *pseudonym*, which prevents users from being tracked across the various dapps they use. A user can create as many identity anchors as they wish.

Unlike most authentication methods, Internet Identity does not require you to set and manage passwords or provide any personal identifying information to dapps or to Internet Identity.

## How Internet Identity works

Internet Identity builds on Web Authentication (WebAuthn) API supported by modern web browsers and operating systems, and the "chain key cryptography" framework that powers the Internet Computer. Essentially, the Internet Computer signs the list of public keys inside the devices assigned to each anchor using its master chain key, which client side code, for example running in the web browser, is aware of.

Dapps that integrate with Internet Identity prompt you to authenticate using an identity anchor. If you don’t have an identity anchor yet, it is easy to create one and add authentication methods to it. For more details, see [How to use Internet Identity](./auth-how-to.md). For each device you add, a pair of cryptographic keys (private and public key) is generated. The public key is stored on the Internet Computer blockchain, while the private key remains locked inside the authentication device together with any biometric data that governs access to it. Adding multiple authentication devices to an identity anchor allows you to access dapps across all of your devices.

When you access a dapp that uses Internet Identity for authentication, you first specify the identity anchor you want to use. After authenticating using an identity anchor using an assigned device, your browser connects to Internet Identity and generates a session key for use with that dapp. Finally, you are asked to authorize access to the dapp.

Your browser downloads the authorization and then redirects you to the dapp. The dapp verifies the authorization from Internet Identity and grants you access as an application-specific anonymous identity that we call pseudonym. Internally, you have a different pseudonym for each dapp, but your pseudonym for any single dapp is the same across all of your devices. All of your devices just represent different methods you can use to authenticate your Internet Identity anchor.

You can register as many identity anchors as you want for redundancy, or different purposes. For example, a user might create one anchor for use with SocialFi or GameFi, and another for use with pure DeFi. They might only feel comfortable adding facial recognition to their SocialFi and GameFi anchor, say, and only use more secure portable HSM devices like YubiKeys and Ledger wallets with their pure DeFi anchor.

## How to use Internet Identity

To learn how to create and use Identity Anchors step-by-step, see [How to use Internet Identity](./auth-how-to.md). This also describes how to set up recovery mechanisms for Identity Anchors.

-->